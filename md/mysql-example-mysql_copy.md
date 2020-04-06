---
title: mysql example mysql copy (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql copy'

Functions in program: 
* `def main(argv):`

Modules used in program: 
* `import mysql.connector`
* `import sys, argparse`

## python mysql copy

Python mysql example: mysql copy

```python
#!/usr/bin/python

import sys, argparse
import mysql.connector

def main(argv):
    parser = argparse.ArgumentParser()
    parser.add_argument("--target", required=True, help="Target db name")
    parser.add_argument("--template", default="template", help="Source template db name")
    args = parser.parse_args()

    template =args.template
    target = args.target

    mysql_optfiles = [
        '/etc/mysqlcopy.cnf',
        '/root/.my.cnf'
    ]   

    print("Copying %s to %s..."%(args.template,args.target))
    cnx = mysql.connector.connect(option_files=mysql_optfiles, option_groups='client',
                                  host='localhost', database=template, charset='utf8')

    c = cnx.cursor()
    c.execute("show tables from %s"%template)   
    tables = []
    for table in c.fetchall():
        tables.append(table[0])

    sql_create_table = ''
    for  table in tables:
        sql_create_table += "CREATE TABLE %(target)s.`%(table)s` LIKE %(template)s.`%(table)s`;"%{'table':table,'target':target,'template':template}

    table_count=0
    for result in c.execute(sql_create_table,multi=True):
       table_count  +=1

    print("Tables created : %d"%table_count)

    sql_copy_data = ''
    for  table in tables:
        sql_copy_data += "INSERT INTO %(target)s.`%(table)s` SELECT * FROM %(template)s.`%(table)s`;"%{'table':table,'target':target,'template':template}

    rows_inserted = 0
    for result in c.execute(sql_copy_data,multi=True):
        rows_inserted += result.rowcount

    print("Rows copied :%d"%rows_inserted)

    c.execute("select rc.CONSTRAINT_NAME, rc.UPDATE_RULE, rc.DELETE_RULE, rc.TABLE_NAME, rc.REFERENCED_TABLE_NAME, kc.COLUMN_NAME, kc.REFERENCED_COLUMN_NAME " + 
             "from INFORMATION_SCHEMA.REFERENTIAL_CONSTRAINTS as rc ,INFORMATION_SCHEMA.KEY_COLUMN_USAGE as kc " + 
             "where rc.CONSTRAINT_SCHEMA='%(template)s' and rc.CONSTRAINT_NAME=kc.CONSTRAINT_NAME and kc.CONSTRAINT_SCHEMA='%(template)s'" % \
             {'template': template})
    sql_create_fkeys = ''
    for fkey in c.fetchall():
        sql_create_fkeys += ("ALTER TABLE `%(target)s`.`%(tbl_name)s` ADD FOREIGN KEY `%(fkey_name)s` (%(index_col_name)s) \
                            REFERENCES `%(target)s`.`%(ref_tbl_name)s` (%(ref_index_col_name)s) ON DELETE %(ondelete_option)s ON UPDATE %(onupdate_option)s;\n" % \
                            {   
                                'target':               target,
                                'fkey_name':            fkey[0],
                                'onupdate_option':      fkey[1],
                                'ondelete_option':      fkey[2],
                                'tbl_name':             fkey[3],
                                'ref_tbl_name':         fkey[4],
                                'index_col_name':       fkey[5],
                                'ref_index_col_name':   fkey[6],
                            })
    fkeys_created = 0
    for result in c.execute(sql_create_fkeys,multi=True):
        fkeys_created += 1

    print("Foreign keys created :%d"%fkeys_created)

    c.execute("select TRIGGER_NAME from information_schema.TRIGGERS where TRIGGER_SCHEMA='%(template)s'" % {'template': template})
    triggers = []
    for trigger in c.fetchall():
        triggers.append(trigger[0])

    sql_create_triggers= ''
    for trigger in triggers:
        c.execute("show create trigger %(trigger)s" % {'trigger': trigger})
        res = c.fetchone()
        sql_create_triggers += res[2] + ";\n"

    cnx.database = target
    c1 = cnx.cursor()
    triggers_created = 0
    for result in c1.execute(sql_create_triggers,multi=True):
        triggers_created += 1

    print("Triggers created :%d" % triggers_created)

    cnx.commit()
    c.close()
    c1.close()
    cnx.close()

if __name__ == "__main__":
   main(sys.argv[1:])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
