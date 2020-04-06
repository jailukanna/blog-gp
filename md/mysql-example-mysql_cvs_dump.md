---
title: mysql example mysql cvs dump (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql cvs dump'

Functions in program: 
* `def main(args):`
* `def mysql_csv_dump(database, outdir, user, password='', host='localhost', column_names=True, verbose=False):`
* `def mysql_escape_keyword(val):`

Modules used in program: 
* `import sys`
* `import re`
* `import csv`
* `import argparse`
* `import mysql.connector`

## python mysql cvs dump

Python mysql example: mysql cvs dump

```python
#!/usr/bin/env python3

import mysql.connector
import argparse
import csv
import re
import sys
from contextlib import closing
from os import makedirs
from os.path import join as join_path
from getpass import getuser, getpass

UNSAFE = re.compile(r'[\[\]/\\;,><&*:%=+@!#^()|?^\0]')

def mysql_escape_keyword(val):
    return '`%s`' % val.replace('`', '``')

def mysql_csv_dump(database, outdir, user, password='', host='localhost', column_names=True, verbose=False):
    if verbose:
        print("creating output directory", outdir)
    makedirs(outdir, exist_ok=True)

    with closing(mysql.connector.connect(host=host, database=database, user=user, password=password)) as con:
        with closing(con.cursor()) as cur:
            cur.execute("show tables")
            for table_name, in cur.fetchall():
                if verbose:
                    print("dumping table", table_name)
                cur.execute("show columns from " + mysql_escape_keyword(table_name))
                order = []
                columns = []
                for column_name, column_type, nullable, key, default, extra in cur.fetchall():
                    if key == 'PRI':
                        order.append(column_name)
                    columns.append(column_name)

                if order:
                    cur.execute("select {columns} from {table} order by {order}".format(
                        columns=', '.join(mysql_escape_keyword(column) for column in columns),
                        table=mysql_escape_keyword(table_name),
                        order=', '.join(mysql_escape_keyword(column) for column in order)))
                else:
                    cur.execute("select {columns} from {table}".format(
                        columns=', '.join(mysql_escape_keyword(column) for column in columns),
                        table=mysql_escape_keyword(table_name)))

                rows = cur.fetchall()

                with open(join_path(outdir, UNSAFE.sub('_', table_name) + '.csv'), 'w') as fp:
                    writer = csv.writer(fp)
                    if column_names:
                        writer.writerow(columns)
                    writer.writerows(rows)
    if verbose:
        print("done")

def main(args):
    parser = argparse.ArgumentParser()
    parser.add_argument('database')
    parser.add_argument('-C', '--out-dir', default='.')
    parser.add_argument('-u', '--user', default=getuser())
    passwords = parser.add_mutually_exclusive_group()
    passwords.add_argument('-p', '--password', default='')
    passwords.add_argument('--prompt-password', action='store_true', default=False)
    parser.add_argument('--host', default='localhost')
    parser.add_argument('--column-names', action='store_true', default=False)
    parser.add_argument('-v', '--verbose', action='store_true', default=False)

    args = parser.parse_args(args)

    if args.prompt_password:
        password = getpass()
    else:
        password = args.password

    mysql_csv_dump(args.database, args.out_dir, args.user, password, args.host, args.column_names, args.verbose)

if __name__ == '__main__':
    main(sys.argv[1:])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
