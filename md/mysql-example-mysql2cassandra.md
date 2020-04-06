---
title: mysql example mysql2cassandra (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql2cassandra'

Functions in program: 
* `def cassandra_import(colnames, f, delim):`
* `def mysql_dump(colnames, q, f, delim):`
* `def cf_cast(cfmetadata, colname, value):`
* `def init_cassandra(params, model):`
* `def init_mysql(params):`
* `def construct_query(colnames, table, criteria):`
* `def process_choice():`
* `def IsNotNull(value):`

Modules used in program: 
* `import MySQLdb, pycassa`
* `import sys, csv`

## python mysql2cassandra

Python mysql example: mysql2cassandra

```python
#! /usr/bin/env python

#
# mysql2cassandra.py
#   Dump a MySQL result set to file and then import into a Cassandra column family
# 
# Configuration
#   mysql_params		[host, port, user, password, db]		MySQL conenction parameters
#   mysql_columns		[colname, colname2, ...]				Columns for building MySQL query
#																	The column that will hold values of the row key in the Cassandra column family must be first
#   mysql_table			'tablename'								Table name for use in MySQL query
#   mysql_criteria		'WHERE col=val ...'						Additional criteria when constructing MySQL query (without ending semicolon)
#   cassandra_params	[host, port, user, password]			Cassandra connection parameters
#   cassandra_model		[keyspace, column_family, row_key]		Cassandra keyspace/CF to insert into and which MySQL column name to use as the value of the row key
#	output_file			"/path/to/data.csv"						Path in which to save the result set dump to
#	validation			[0 | 1]									Disable or enable validation of data types before attempting to dump and import 
#																	Prompts for specific changes will still be given
#
#	debug				[0 | 1]									Disable or enable debug information in STDOUT
#
# Validation
#	This step is recommended to be manually done, if necessary by a database administrator due to limitations in this script:
#		An example being that TINYINT(1) will be added as IntegerType rather than BooleanType
#	Attempts to modify or add metadata to the column family will take place if:
#		a column in the MySQL query is not present in the Cassandra column family
#		a column's data type in the MySQL table structure does not match the Cassandra column family
#
# Import process
#	The first column in the MySQL query will be used as the row key for each insertion into the Cassandra column family
#   If row values of columns in the MySQL resultset are empty, they will be omitted from the insert into the Cassandra column family
#   
# Dependencies
#   mysqldb
#   pycassa


#
# Script configuration options
#

mysql_params = ['', 3306, '', '', '']
mysql_columns = ['id', 'server_ref', 'serverversion', 'systemtime', 'uptime', 'diskfreespace']
mysql_table = 'serverstatuses_history'
mysql_criteria = ''

cassandra_params = ['10.11.11.11', 9160, '', '']
cassandra_model = ['demo', 'serverstatuses_history', 'server_ref']

output_file = "./mysql.csv"
validation = 1

debug = 1


import sys, csv
from distutils.util import strtobool
import MySQLdb, pycassa
from MySQLdb import cursors

# MySQL to Cassandra datatype mappings
datatype_map = {
	'bit': 'BooleanType',
	'tinyint': 'IntegerType',
	'smallint': 'IntegerType',
	'int': 'IntegerType',
	'mediumint': 'IntegerType',
	'bigint': 'LongType',
	'decimal': 'DecimalType',
	'float': 'FloatType',
	'double': 'DoubleType',
	'char': 'UTF8Type',
	'varchar': 'UTF8Type',
	'text': 'UTF8Type',
	'blob': 'BytesType',
	'timestamp': 'DateType',
	'datetime': 'DateType',
	'date': 'DateType',
	'time': 'UTF8Type',
	'year': 'UTF8Type'
}

# Utility functions
def IsNotNull(value):
    return value is not None and len(value) > 0

def process_choice():
	while True:
		try:
			choice = strtobool(raw_input())
			break
		except ValueError:
			sys.stdout.write("What was that? [y/n] ")
			
	return choice
	
def construct_query(colnames, table, criteria):
	q = 'SELECT '+(', '.join(colnames))+' FROM '+table
	if IsNotNull(criteria): 
		q += ' '+criteria+';'
	else:
		q += ';'
		
	return q

# MySQL connection initialization
def init_mysql(params):
	try:
		mcon = MySQLdb.connect(host=params[0], port=params[1], user=params[2], passwd=params[3], db=params[4], use_unicode=True, cursorclass = MySQLdb.cursors.SSCursor)
		mcur = mcon.cursor()
	except MySQLdb.Error, e:
		print("Error %d: %s" % (e.args[0],e.args[1]))
		sys.exit(1)
	
	mcur.execute("SET NAMES 'utf8';")
	mcur.execute("SET @@NET_WRITE_TIMEOUT = 900;")
	return mcur, mcon

# Cassandra connection initialization
def init_cassandra(params, model):
	host = params[0]+':'+str(params[1])
	if IsNotNull(params[2]) and IsNotNull(params[3]):
		credentials = {'username': params[2], 'password': params[3]}
	else:
		credentials = None
	
	ccon = pycassa.pool.ConnectionPool(keyspace=model[0], server_list=[host], credentials=credentials)
	ccur = pycassa.ColumnFamily(ccon,model[1])
	csys = pycassa.system_manager.SystemManager(host)
	return ccur, csys, ccon

# Cassandra - Column metadata alteration
def	cf_alter_metadata(key, datatype):
	if cassandra_datatypes.get(key) is False:
		sys.stdout.write("[?] Add column '"+key+"' as data type '"+datatype+"'? [y/n] ")
	else:
		odatatype = cassandra_datatypes.get(key)
		sys.stdout.write("[?] Alter column '"+key+"' from data type '"+odatatype+"' to '"+datatype+"'? [y/n] ")
		
	choice = process_choice()
	if choice:
		csys.alter_column(cassandra_model[0], cassandra_model[1], key, datatype)

# cast a column value as the data type that matches its validator in the Cassandra column family
def cf_cast(cfmetadata, colname, value):
	validator = cfmetadata[colname]
		
	if validator == 'IntegerType':
		value = int(value)
	elif validator == 'FloatType' or validator == 'DoubleType':
		value = float(value)
	elif validator == 'BytesType':
		q = "SELECT character_set_name FROM information_schema.columns WHERE table_schema = '"+mysql_params[4]+"' AND table_name = '"+mysql_table+"' AND column_name = '"+colname+"';"
		mcur.execute(q)
		charset = mcur.fetchone()
		value = bytesarray(value, charset[0])
		
	return value
		
# Write MySQL result set to CSV line by line
def mysql_dump(colnames, q, f, delim):
	print("[!] Executing MySQL query")
	mcur.execute(q)

	file = open(f, 'w')
	writer = csv.writer(file, delimiter = delim, quotechar = '"', quoting = csv.QUOTE_MINIMAL)
	print("[!] Writing result set to file '"+output_file+"'")
	while True:
		# Get one record from result set
		row = mcur.fetchone()
		# If no records left, break loop
		if not row:
			break
		
		# Write record to file in comma-delimited format
		writer.writerow(row)
		
	file.close()

# Read CSV file line by line and insert into Cassandra column family
def cassandra_import(colnames, f, delim):
	rownum = 0
	file = open(f, 'r')
	reader = csv.reader(file, delimiter = delim, lineterminator = '\n')
	print("[!] Reading file and inserting records into Cassandra")
	# Read the CSV line by line
	for row in reader:
		coldat = dict()
		# Get the number of columns, excluding the row key
		colcount = len(colnames) - 1
		
		# Iterate each column, adding the column name-value pair to a dictionary
		for i in range(colcount):
			i += 1
			# Skip columns that are empty
			if IsNotNull(row[i]):
				# cast the type of the value for this column according to the column metadata
				colvar = cf_cast(cassandra_datatypes, colnames[i], row[i])
				coldat[colnames[i]] = colvar
				#print(row[0], colnames[i], row[i])
				
		# Insert the data into the Cassandra column family
		try:
			ccur.insert(row[0],coldat)
			if debug == 1:
				print("[*] Inserted row with key "+row[0])
		except:
			print("[*] Failed to insert row with key "+row[0])
			
		rownum += 1
	
	file.close()
	return rownum

	
mcur, mcon = init_mysql(mysql_params)
ccur, csys, ccon = init_cassandra(cassandra_params, cassandra_model)
	
# Data type validation 
if validation:
	print("[!] Beginning data type validation")

	# Get the data types of the columns in the MySQL table
	q = "SELECT column_name, data_type FROM information_schema.columns WHERE table_schema = '"+mysql_params[4]+"' AND table_name = '"+mysql_table+"' AND column_name IN ("+(', '.join('"{0}"'.format(col) for col in mysql_columns))+");"
	mysql_datatypes = dict()
	mcur.execute(q)
	for row in mcur:
		mysql_datatypes[row[0]] = row[1]
	
	if len(mysql_datatypes) != len(mysql_columns):
		print("[?] A number of given columns for MySQL table ("+mysql_table+") does not match information_schema result - you may have typo'd a column in the list")
		sys.exit(1)

	# Do the same for the Cassandra column family keys
	cassandra_datatypes = dict()
	cassandra_datatypes[mysql_columns[0]] = ccur.key_validation_class
	for key, val in ccur.column_validators.items():
		cassandra_datatypes[key] = val
	
	# If number of MySQL columns given do not match what is defined in the Cassandra column family metadata, ask the user if the script should alter the column family
	if len(cassandra_datatypes) != len(mysql_columns):
		print("[~] A number of MySQL columns specified do not match the number of column's defined in the Cassandra column family ("+cassandra_model[1]+") metadata")
		sys.stdout.write("[?] Determine the missing column metadata and modify the column family? (a prompt will be given before each addition) [y/n] ")
		choice = process_choice()
		if choice:
			for mkey, mtype in mysql_datatypes.items():
				if cassandra_datatypes.get(mkey) is False:
					cf_alter_metadata(mkey, mtype)
					
	# If the data types between MySQL and Cassandra columns do not match, ask the user if the script should modify their definitions
	metadata_changes = dict()
	for mkey, mtype in mysql_datatypes.items():
		# Don't check the key row key data type
		if mkey != mysql_columns[0]:
			if datatype_map.get(mtype) != cassandra_datatypes.get(mkey):
				metadata_changes[mkey] = datatype_map.get(mtype)
	
	if len(metadata_changes) > 0:
		print("[~] Some Cassandra column metadata have been determined to be an inappropriate match, given the MySQL columns' datatypes")
		sys.stdout.write("[?] Review the Cassandra columns that are proposed to have their data types changed? [y/n] ")
		choice = process_choice()
		if choice:
			for key, type in metadata_changes.items():
				cf_alter_metadata(key, type)

mysql_query = construct_query(mysql_columns, mysql_table, mysql_criteria)
print("[~] Below is the MySQL query constructed from the given configuration:")
print(mysql_query)
sys.stdout.write("[?] Is this correct? [y/n] ")
choice = process_choice()
if choice is False:
	sys.exit(1)

print("[!] Working... this may take awhile!\n")

# Stream MySQL result set to file
mysql_dump(mysql_columns, mysql_query, output_file, ',')
print("[!] Completed MySQL dump")

# Import dump into Cassandra row by row 
insertions = cassandra_import(mysql_columns, output_file, ',')
print("[!] Completed Cassandra import ("+str(insertions)+" rows processed)")

ccon.dispose()
mcon.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
