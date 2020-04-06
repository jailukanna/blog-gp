---
title: mysql example dbcopier (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'dbcopier'

Functions in program: 
* `def get_dbcopy():`
* `def combine_files(self,file1,file2):`

Modules used in program: 
* `import getpass`
* `import time`
* `import os`
* `import ConfigParser`

## python dbcopier

Python mysql example: dbcopier

```python
#!/usr/bin/env python
import ConfigParser
import os
import time
import getpass


def combine_files(self,file1,file2):
    with open('file1', 'w') as outfile:
            with open(file2) as infile:
                for line in infile:
                    outfile.write(line)    

def get_dbcopy():
    
    script_dir = os.path.dirname(os.path.realpath(__file__))    

    print("Enter origin database user:")
    origin_user = raw_input()

    print("Enter origin database password: (Password will not be visible)")
    origin_password = getpass.getpass()

    print("Enter origin database host:")
    origin_host = raw_input()

    print("Enter origin database: (Which database to dump")
    origin_database = raw_input()

    print("Enter destination database user:")
    dest_user = raw_input()

    print("Enter destination database password: (Password will not be visible)")
    dest_password = getpass.getpass()

    print("Enter destination database host: (leave blank for same host)")
    dest_host = raw_input()
    if(not dest_host):
        dest_host = origin_host
    
    print("Enter destination database name:")
    dest_database = raw_input()
    
    print("Include routines? y/n (leave blank for y)")
    include_routines = raw_input()
    
    if(not include_routines):
        include_routines = "y"


    filestamp = time.strftime('%Y-%m-%d-%I:%M')
    dump_file = script_dir+"/"+origin_database+"_"+filestamp+".sql"
    if(include_routines.lower() == "y" or include_routines == ''):
        os.popen("mysqldump -u %s --password%s -h %s --default-character-set=utf8 --routines --ignore-table=%s.log %s --result-file $s" % (origin_user,password,origin_host,origin_database,origin_database,dump_file))
    else:
        os.popen("mysqldump -u %s --password%s -h %s --default-character-set=utf8 --ignore-table=%s.log %s --result-file $s" % (origin_user,origin_password,origin_host,origin_database,origin_database,dump_file))

    
    print("\n-- The dump file has been created @ "+dump_file+" --")
    
    sql_update_file = script_dir+"/"+database+"_updates.sql";
    
    #if sql_update_file exists combine with sql dump file
    if(os.path.isfile(sql_update_file)):
        print("\n-- combing the dump file with: "+sql_update_file)
        combine_files(dump_file,sql_update_file)
        
    
    #import mysql dump file
    os.popoen("mysql -u %s --password%s -h %s --default-character-set=utf8 %s < %s" % (dest_user,dest_password,dest_host,dest_database,dump_file))
    
if __name__=="__main__":
    get_dbcopy()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
