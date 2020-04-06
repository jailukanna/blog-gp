---
title: mysql example wrapper (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'wrapper'


Modules used in program: 
* `import datetime`
* `import psutil`
* `import sys`
* `import argparse         #if needed later`
* `import configparser`
* `import os`
* `import subprocess`

## python wrapper

Python mysql example: wrapper

```python
import subprocess
import os
import configparser
import argparse         #if needed later
import sys
import psutil
import datetime

config = configparser.ConfigParser()
descr="Backup postgresql/mysql databases\n INFORMATION:\n\nDepending on your pg_hba configuration a .pgpass file may be needed - it has to be present in the home directory(of the user running the script - if a password is requried)\nExample .pgpass file:localhost:5432:database_name:user:password\n .pgpass permissions have to be 0600!\n\nInteractive = yes means that -W option will be used for pg_dump commands. if Interactive is not set to yes -w will be used on all pg_dump commands(no password prompt, you either have access or a .pgpass file in home dir"
parser = argparse.ArgumentParser(description=descr) ##gets argument(filename)
parser.add_argument('filename', type=str,help="config file with backup options(full path unless in same directory)")
args = parser.parse_args()
try:
    a=config.read(args.filename)
    for database in config.get("DEFAULT", "db_name").split("\n"):
            print("Using db: "+database)
            if(config["DEFAULT"]["backup_db_type"]=="postgresql"):
                if(config["DEFAULT"]["remote_db"]=="yes"):
                    import remote
                    exit_value=remote.main(config,database)
                    
                if(config["DEFAULT"]["backup_remotely"]=="yes"):
                    import from_remote_to_local
                    exit_value=from_remote_to_local.main(config,database)

                else:
                    import backup
                    exit_value=backup.main(config,database)  ##postgresql
        
            else:
                if(config["DEFAULT"]["remote_db"]=="yes"):
                    import remote_mysql
                    exit_value=remote_mysql.main(config,database)
                
                else:    
                    import mysql
                    exit_value=mysql.main(config,database)
    if(exit_value!=0):
        print("Internal ERROR: Exit value was !=0")

                    
except configparser.NoOptionError:
    sys.stderr.write("Erorr: Invalid/no option found for a row in config file")
    sys.exit(1)
except configparser.NoSectionError:
    sys.stderr.write("Invalid section found in config file")
    sys.exit(1)
except configparser.ParsingError:
    sys.stderr.write("Unable to parse given config file")
    sys.exit(1)
except configparser.Error:
    sys.stderr.write("Unable to access config file."+args.filename)
    sys.exit(1)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
