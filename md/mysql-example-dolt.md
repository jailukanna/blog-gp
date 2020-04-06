---
title: mysql example dolt (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'dolt'

Functions in program: 
* `def exec_test_query(dr, query_str):`
* `def _execute_restart_serve_if_needed(dlt, args):`
* `def _execute(args, cwd):`

Modules used in program: 
* `import time`
* `import os`
* `import pandas`
* `import mysql.connector`

## python dolt

Python mysql example: dolt

```python
import mysql.connector
import pandas
import os
import time

from subprocess import *


class DoltException(Exception):
    def __init__(self, exec_args, stdout, stderr, exitcode):
        self.exec_args = exec_args
        self.stdout = stdout
        self.stderr = stderr
        self.exitcode = exitcode


def _execute(args, cwd):
    proc = Popen(args=args, cwd=cwd, stdout=PIPE, stderr=PIPE)
    out, err = proc.communicate()
    exitcode = proc.returncode

    if exitcode != 0:
        raise DoltException(args, out, err, exitcode)

def _execute_restart_serve_if_needed(dlt, args):
    was_serving = False
    if dlt.server is not None:
        was_serving = True
        dlt.stop_server()

    _execute(args=args, cwd=dlt.repo_dir)

    if was_serving:
        dlt.start_server()


class Dolt(object):
    def __init__(self, repo_dir):
        self.repo_dir = repo_dir
        self.dir_exists = os.path.exists(repo_dir) and os.path.exists(repo_dir)
        self.server = None
        self.cnx = None

    def config(self, is_global, user_name, user_email):
        args = ["dolt", "config", "add"]

        if is_global:
            args.append("--global")
        elif not self.dir_exists:
            raise Exception(self.repo_dir + " does not exist.  Cannot configure local options without a valid directory.")

        name_args = args
        email_args = args.copy()

        name_args.append(["user.name", user_name])
        email_args.append(["user.email", user_email])

        if is_global:
            _execute(args=name_args, cwd=None)
            _execute(args=email_args, cwd=None)

        else:
            _execute(args=name_args, cwd=self.repo_dir)
            _execute(args=email_args, cwd=self.repo_dir)

    def clone(self, repo):
        if self.dir_exists:
            raise Exception(self.repo_dir + " .")

        os.makedirs(self.repo_dir)
        self.dir_exists = True

        args = ["dolt", "clone", repo, "./"]

        _execute(args=args, cwd=self.repo_dir)

    def init_new_repo(self):
        args = ["dolt", "init"]

        _execute(args=args, cwd=self.repo_dir)

    def create_branch(self, branch_name, commit_ref=None):
        args = ["dolt", "branch", branch_name]

        if commit_ref is not None:
            args.append(commit_ref)

        _execute(args=args, cwd=self.repo_dir)

    def checkout(self, branch_name):
        args = ["dolt", "checkout", branch_name]
        _execute_restart_serve_if_needed(self, args)

    def start_server(self):
        if self.server is not None:
            raise Exception("already running")

        args = ['dolt', 'sql-server', '-t', '0']
        proc = Popen(args=args, cwd=self.repo_dir, stdout=PIPE, stderr=STDOUT)

        # should probably start a thread to watch this or something.

        # hack: need to wait for the process to get going otherwise the connection will fail
        time.sleep(0.5)

        cnx = mysql.connector.connect(user='root', host='127.0.0.1', port=3306, database='dolt')

        self.server = proc
        self.cnx = cnx

    def stop_server(self):
        if self.server is None:
            raise Exception("never started.")

        self.cnx.close()
        self.cnx = None

        self.server.kill()
        self.server = None

    def query_server(self, query):
        if self.server is None:
            raise Exception("never started.")

        cursor = self.cnx.cursor()
        cursor.execute(query)

        return cursor

    def pandas_read_sql(self, query):
        if self.server is None:
            raise Exception("never started.")

        df = pandas.read_sql(query, con=self.cnx)

        return df

    def put_row(self, table_name, row_data):
        args = ["dolt", "table", "put-row", table_name]
        key_value_pairs = [str(k) + ':' + str(v) for k, v in row_data.items()]
        args.extend(key_value_pairs)

        _execute_restart_serve_if_needed(self, args)

    def add_table_to_next_commit(self, table_name):
        args = ["dolt", "add", table_name]
        _execute_restart_serve_if_needed(self, args)

    def commit(self, commit_message):
        args = ["dolt", "commit", "-m", commit_message]
        _execute_restart_serve_if_needed(self, args)


def exec_test_query(dr, query_str):
    query_cursor = dr.query_server(query_str)

    for row in query_cursor:
        print(' | '.join([str(col_val) for col_val in row]))


DIR = '/users/brian/dolt-python-test/'
QUERY = '''SELECT * 
FROM tracks 
WHERE artist_name = "Nirvana" 
LIMIT 10;'''

dolt_repo = Dolt(DIR)
# dolt_repo.clone("bheni/million-songs")
dolt_repo.start_server()

try:
    exec_test_query(dolt_repo, QUERY)

    dolt_repo.create_branch("python-branch", "master")
    dolt_repo.checkout("python-branch")
    exec_test_query(dolt_repo, QUERY)

except Exception as e:
    print(e)

dolt_repo.stop_server()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
