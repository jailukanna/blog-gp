---
title: mysql example sqltest (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'sqltest'

Functions in program: 
* `def create_db(db_cursor):`

Modules used in program: 
* `import mysql.connector	# for connecting to mysql servers using python`

## python sqltest

Python mysql example: sqltest

```python
# connect to MySQL:

import mysql.connector	# for connecting to mysql servers using python
from mysql.connector import errorcode	# for exception handling
from datetime import date, datetime, timedelta

try:

	connect1= mysql.connector.connect(user= 'root', password= 'password', host= '127.0.0.1')	# 127.0.0.1= localhost connection

	# for future connections after database is created: connect1= mysql.connector.connect(user= 'root', password= 'password', host= '127.0.0.1', database= 'database_name')

	print("Database connection OK.")

except mysql.connector.Error as connect_error:

	if connect_error.errno == errorcode.ER_ACCESS_DENIED_ERROR:
		print("User or password error, try again.")

	elif connect_error.errno == errorcode.ER_BAD_DB_ERROR:
		print("Database does not exist, try again.")

	else:
		print(err)



db_cursor= connect1.cursor()	# data structure used for MySQL databases in python


# create the "employees" database:

DB_NAME= 'employees'

def create_db(db_cursor):

	try:
		db_cursor.execute("CREATE DATABASE {} DEFAULT CHARACTER SET 'utf8'".format(DB_NAME))	# creates the database defined by DB_NAME as 'employees'
	except mysql.connector.Error as db_err:
		print("Failed creating database: {}".format(db_err))
		exit(1)

try:
	db_cursor.execute("USE {}".format(DB_NAME))		# selects the just created database as the current database

except mysql.connector.Error as db_err:
	print("Database {} does not exist.".format(DB_NAME))

	if db_err.errno == errorcode.ER_BAD_DB_ERROR:
		create_db(db_cursor)	# creates the database stored in DB_NAME if the database does not exist
		print("Database {} created successfully.".format(DB_NAME))
		connect1.database= DB_NAME

	else:
		print(db_err)
		exit(1)


# create tables in the "employees" database after connecting to the MySQL database:


TABLES = {}		# a dictionary data structure is created as an efficient way of storing the different tables in a database. The table name is the "key" and the "value" for each key is the mysql commands for creating the various rows and their data types

TABLES['employees'] = ("CREATE TABLE `employees` (`emp_no` int(11) NOT NULL AUTO_INCREMENT, `birth_date` date NOT NULL, `first_name` varchar(14) NOT NULL, `last_name` varchar(16) NOT NULL, `gender` enum('M', 'F') NOT NULL, `hire_date` date NOT NULL, PRIMARY KEY (`emp_no`)) ENGINE=InnoDB")

TABLES['departments'] = ("CREATE TABLE `departments` (`dept_no` char(4) NOT NULL, `dept_name` varchar(40) NOT NULL, PRIMARY KEY (`dept_no`), UNIQUE KEY `dept_name` (`dept_name`)) ENGINE=InnoDB")

TABLES['salaries'] = ("CREATE TABLE `salaries` (`emp_no` int(11) NOT NULL, `salary` int(11) NOT NULL, `from_date` date NOT NULL, `to_date` date NOT NULL, PRIMARY KEY (`emp_no`,`from_date`), KEY `emp_no` (`emp_no`), CONSTRAINT `salaries_ibfk_1` FOREIGN KEY (`emp_no`) REFERENCES `employees` (`emp_no`) ON DELETE CASCADE) ENGINE=InnoDB")

TABLES['dept_emp'] = ("CREATE TABLE `dept_emp` (`emp_no` int(11) NOT NULL, `dept_no` char(4) NOT NULL, `from_date` date NOT NULL, `to_date` date NOT NULL, PRIMARY KEY (`emp_no`,`dept_no`), KEY `emp_no` (`emp_no`), KEY `dept_no` (`dept_no`), CONSTRAINT `dept_emp_ibfk_1` FOREIGN KEY (`emp_no`) REFERENCES `employees` (`emp_no`) ON DELETE CASCADE, CONSTRAINT `dept_emp_ibfk_2` FOREIGN KEY (`dept_no`) REFERENCES `departments` (`dept_no`) ON DELETE CASCADE) ENGINE=InnoDB")

TABLES['dept_manager'] = ("CREATE TABLE `dept_manager` (`dept_no` char(4) NOT NULL, `emp_no` int(11) NOT NULL, `from_date` date NOT NULL, `to_date` date NOT NULL, PRIMARY KEY (`emp_no`,`dept_no`), KEY `emp_no` (`emp_no`), KEY `dept_no` (`dept_no`), CONSTRAINT `dept_manager_ibfk_1` FOREIGN KEY (`emp_no`) REFERENCES `employees` (`emp_no`) ON DELETE CASCADE, CONSTRAINT `dept_manager_ibfk_2` FOREIGN KEY (`dept_no`) REFERENCES `departments` (`dept_no`) ON DELETE CASCADE) ENGINE=InnoDB")

TABLES['titles'] = ("CREATE TABLE `titles` (`emp_no` int(11) NOT NULL, `title` varchar(50) NOT NULL, `from_date` date NOT NULL, `to_date` date DEFAULT NULL, PRIMARY KEY (`emp_no`,`title`,`from_date`), KEY `emp_no` (`emp_no`), CONSTRAINT `titles_ibfk_1` FOREIGN KEY (`emp_no`) REFERENCES `employees` (`emp_no`) ON DELETE CASCADE) ENGINE=InnoDB")


# iterate over the tables just created to add them to the 'employees' database:

for table_name in TABLES:
	table_description= TABLES[table_name]

	try:
		print("Creating table {}: ".format(table_name), end='')		# user friendly print(function will display what tables have been created)
		db_cursor.execute(table_description)

	except mysql.connector.Error as table_err:
		if table_err.errno == errorcode.ER_TABLE_EXISTS_ERROR:
			print("Table already exists: {} ".format(table_name))
		else:
			print(table_err.msg)
	else:
		print("OK.")


# insert data into the newly created tables:

all_employees= {}	# initialize an employees dictionary to store all employees and salaries
all_salaries= {}
emp_no= 0

all_employees['George Washington']= ('George', 'Washington', date(2000, 6, 14), 'M', date(1732, 2, 22))
all_employees['John Adams']= ('John', 'Adams', date(2001, 1, 27), 'M', date(1735, 10, 30))

all_salaries['George Washington']= {'emp_no': emp_no, 'salary': 50000, 'from_date': date(2000, 6, 14), 'to_date': date(9999, 1, 1)}
all_salaries['John Adams']= {'emp_no': emp_no, 'salary': 45000, 'from_date': date(2001, 1, 27), 'to_date': date(9999, 1, 1)}

add_employee= ("INSERT INTO employees (first_name, last_name, hire_date, gender, birth_date) VALUES (%s, %s, %s, %s, %s)")

add_salary= ("INSERT INTO salaries (emp_no, salary, from_date, to_date) VALUES (%(emp_no)s, %(salary)s, %(from_date)s, %(to_date)s)")




for employee in all_employees:


	try:
		print("Adding employee {}: ".format(employee), end='')
		db_cursor.execute(add_employee, all_employees[employee])
		emp_no= db_cursor.lastrowid 	# cursor.lastrowid returns the AUTO_INCREMENT value for the last executed row. In this case it was "emp_no" from the employees table. We need this value as it is a primary key and is used in the next table "salaries"
		db_cursor.execute(add_salary, all_salaries[employee])

	except mysql.connector.Error as data_err:
		print(data_err.msg)
	else:
		print("Employee and Salary OK.")












connect1.commit()		# all data must be committed after being executed so that it is properly stored in the database










db_cursor.close()	# close the MySQL cursor when finished inputting data into the database








connect1.close()	# always close the connection when done with mysql

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
