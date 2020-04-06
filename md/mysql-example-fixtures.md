---
title: mysql example fixtures (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'fixtures'

Functions in program: 
* `def db_is_present(providers, config):`
* `def do_rollback(test):`
* `def logging_context(test):`

Modules used in program: 
* `import signal`
* `import threading`
* `import unittest`
* `import click`
* `import logging`
* `import os`
* `import sys`

## python fixtures

Python mysql example: fixtures

```python
import sys
import os
import logging

from pony.py23compat import PY2
from ponytest import with_cli_args, pony_fixtures, ValidationError, Fixture, provider_validators, provider

from functools import wraps, partial
import click
from contextlib import contextmanager, closing


from pony.orm.dbproviders.mysql import mysql_module
from pony.utils import cached_property, class_property

if not PY2:
    from contextlib import contextmanager, ContextDecorator, ExitStack
else:
    from contextlib2 import contextmanager, ContextDecorator, ExitStack

import unittest

from pony.orm import db_session, Database, rollback, delete

if not PY2:
    from io import StringIO
else:
    from StringIO import StringIO

from multiprocessing import Process

import threading


class DBContext(ContextDecorator):

    __fixture__ = 'db'

    enabled = False

    def __init__(self, Test):
        if not isinstance(Test, type):
            TestCls = type(Test)
            NewClass = type(TestCls.__name__, (TestCls,), {})
            NewClass.__module__ = TestCls.__module__
            NewClass.db = property(lambda t: self.db)
            Test.__class__ = NewClass
        else:
            Test.db = class_property(lambda cls: self.db)
        Test.db_provider = self.provider
        self.Test = Test

    @class_property
    def fixture_name(cls):
        return cls.provider

    @class_property
    def provider(cls):
        # is used in tests
        return cls.PROVIDER

    def init_db(self):
        raise NotImplementedError

    @cached_property
    def db(self):
        raise NotImplementedError

    def __enter__(self):
        self.init_db()
        try:
            self.Test.make_entities()
        except (AttributeError, TypeError):
            # No method make_entities with due signature
            pass
        else:
            self.db.generate_mapping(check_tables=True, create_tables=True)
        return self.db

    def __exit__(self, *exc_info):
        self.db.provider.disconnect()

    # @classmethod
    # @with_cli_args
    # @click.option('--db', '-d', 'database', multiple=True)
    # @click.option('--exclude-db', '-e', multiple=True)
    # def cli(cls, database, exclude_db):
    #     fixture = [
    #         MySqlContext, OracleContext, SqliteContext, PostgresContext,
    #         SqlServerContext,
    #     ]
    #     all_db = [ctx.provider for ctx in fixture]
    #     for db in database:
    #         if db == 'all':
    #             continue
    #         assert db in all_db, (
    #             "Unknown provider: %s. Use one of %s." % (db, ', '.join(all_db))
    #         )
    #     if 'all' in database:
    #         database = all_db
    #     elif exclude_db and not database:
    #         database = set(all_db) - set(exclude_db)
    #     elif not database:
    #         database = ['sqlite']
    #     for Ctx in fixture:
    #         if Ctx.provider in database:
    #             yield Ctx

    db_name = 'testdb'

# class DbFixture(Fixture):
#     __key__ = 'db'


# class GenerateMapping(Fixture):
#     __key__ = 'generate_mapping'


@provider()
class GenerateMapping(ContextDecorator):

    weight = 200
    scope = 'class'
    __fixture__ = 'generate_mapping'

    def __init__(self, Test):
        self.Test = Test

    def __enter__(self):
        db = getattr(self.Test, 'db', None)
        if not db or not db.entities:
            return
        for entity in db.entities.values():
            if entity._database_.schema is None:
                db.generate_mapping(check_tables=True, create_tables=True)
            break

    def __exit__(self, *exc_info):
        pass

@provider()
class MySqlContext(DBContext):
    PROVIDER  = 'mysql'

    def drop_db(self, cursor):
        cursor.execute('use sys')
        cursor.execute('drop database %s' % self.db_name)


    def init_db(self):
        with closing(mysql_module.connect(**self.CONN).cursor()) as c:
            try:
                self.drop_db(c)
            except mysql_module.DatabaseError as exc:
                print('Failed to drop db: %s' % exc)
            c.execute('create database %s' % self.db_name)
            c.execute('use %s' % self.db_name)

    CONN = {
        'host': "localhost",
        'user': "ponytest",
        'passwd': "ponytest",
    }

    @cached_property
    def db(self):
        CONN = dict(self.CONN, db=self.db_name)
        return Database('mysql', **CONN)

@provider()
class SqlServerContext(DBContext):

    PROVIDER = 'sqlserver'

    def get_conn_string(self, db=None):
        s = (
            'DSN=MSSQLdb;'
            'SERVER=mssql;'
            'UID=sa;'
            'PWD=pass;'
        )
        if db:
            s += 'DATABASE=%s' % db
        return s

    @cached_property
    def db(self):
        CONN = self.get_conn_string(self.db_name)
        return Database('mssqlserver', CONN)

    def init_db(self):
        import pyodbc
        cursor = pyodbc.connect(self.get_conn_string(), autocommit=True).cursor()
        with closing(cursor) as c:
            try:
                self.drop_db(c)
            except pyodbc.DatabaseError as exc:
                print('Failed to drop db: %s' % exc)
            c.execute('create database %s' % self.db_name)
            c.execute('use %s' % self.db_name)

    def drop_db(self, cursor):
        cursor.execute('use master')
        cursor.execute('drop database %s' % self.db_name)


@provider()
class SqliteContext(DBContext):
    PROVIDER = 'sqlite'
    enabled = True

    def init_db(self):
        try:
            os.remove(self.db_path)
        except OSError as exc:
            print('Failed to drop db: %s' % exc)


    @cached_property
    def db_path(self):
        p = os.path.dirname(__file__)
        p = os.path.join(p, '%s.sqlite' % self.db_name)
        return os.path.abspath(p)

    @cached_property
    def db(self):
        return Database('sqlite', self.db_path, create_db=True)


@provider()
class PostgresContext(DBContext):
    PROVIDER = 'postgresql'

    def get_conn_dict(self, no_db=False):
        d = dict(
            user='ponytest', password='ponytest',
            host='localhost', database='postgres',
        )
        if not no_db:
            d.update(database=self.db_name)
        return d

    def init_db(self):
        import psycopg2
        conn = psycopg2.connect(
            **self.get_conn_dict(no_db=True)
        )
        conn.set_isolation_level(0)
        with closing(conn.cursor()) as cursor:
            try:
                self.drop_db(cursor)
            except psycopg2.DatabaseError as exc:
                print('Failed to drop db: %s' % exc)
            cursor.execute('create database %s' % self.db_name)

    def drop_db(self, cursor):
        cursor.execute('drop database %s' % self.db_name)


    @cached_property
    def db(self):
        return Database('postgres', **self.get_conn_dict())


@provider()
class OracleContext(DBContext):
    PROVIDER = 'oracle'

    def __enter__(self):
        os.environ.update(dict(
            ORACLE_BASE='/u01/app/oracle',
            ORACLE_HOME='/u01/app/oracle/product/12.1.0/dbhome_1',
            ORACLE_OWNR='oracle',
            ORACLE_SID='orcl',
        ))
        return super(OracleContext, self).__enter__()

    def init_db(self):

        import cx_Oracle
        with closing(self.connect_sys()) as conn:
            with closing(conn.cursor()) as cursor:
                try:
                    self._destroy_test_user(cursor)
                except cx_Oracle.DatabaseError as exc:
                    print('Failed to drop user: %s' % exc)
                try:
                    self._drop_tablespace(cursor)
                except cx_Oracle.DatabaseError as exc:
                    print('Failed to drop db: %s' % exc)
                cursor.execute(
                """CREATE TABLESPACE %(tblspace)s
                DATAFILE '%(datafile)s' SIZE 20M
                REUSE AUTOEXTEND ON NEXT 10M MAXSIZE %(maxsize)s
                """ % self.parameters)
                cursor.execute(
                """CREATE TEMPORARY TABLESPACE %(tblspace_temp)s
                TEMPFILE '%(datafile_tmp)s' SIZE 20M
                REUSE AUTOEXTEND ON NEXT 10M MAXSIZE %(maxsize_tmp)s
                """ % self.parameters)
                self._create_test_user(cursor)


    def _drop_tablespace(self, cursor):
        cursor.execute(
            'DROP TABLESPACE %(tblspace)s INCLUDING CONTENTS AND DATAFILES CASCADE CONSTRAINTS'
        % self.parameters)
        cursor.execute(
            'DROP TABLESPACE %(tblspace_temp)s INCLUDING CONTENTS AND DATAFILES CASCADE CONSTRAINTS'
        % self.parameters)


    parameters = {
        'tblspace': 'test_tblspace',
        'tblspace_temp': 'test_tblspace_temp',
        'datafile': 'test_datafile.dbf',
        'datafile_tmp': 'test_datafile_tmp.dbf',
        'user': 'ponytest',
        'password': 'ponytest',
        'maxsize': '100M',
        'maxsize_tmp': '100M',
    }

    def connect_sys(self):
        import cx_Oracle
        return cx_Oracle.connect('sys/the@localhost/ORCL', mode=cx_Oracle.SYSDBA)

    def connect_test(self):
        import cx_Oracle
        return cx_Oracle.connect('ponytest/ponytest@localhost/ORCL')


    @cached_property
    def db(self):
        return Database('oracle', 'ponytest/ponytest@localhost/ORCL')

    def _create_test_user(self, cursor):
        cursor.execute(
        """CREATE USER %(user)s
            IDENTIFIED BY %(password)s
            DEFAULT TABLESPACE %(tblspace)s
            TEMPORARY TABLESPACE %(tblspace_temp)s
            QUOTA UNLIMITED ON %(tblspace)s
        """ % self.parameters
        )
        cursor.execute(
        """GRANT CREATE SESSION,
                    CREATE TABLE,
                    CREATE SEQUENCE,
                    CREATE PROCEDURE,
                    CREATE TRIGGER
            TO %(user)s
        """ % self.parameters
        )

    def _destroy_test_user(self, cursor):
        cursor.execute('''
            DROP USER %(user)s CASCADE
        ''' % self.parameters)


@provider(__fixture__='log', weight=100, enabled=False)
@contextmanager
def logging_context(test):
    level = logging.getLogger().level
    from pony.orm.core import debug, sql_debug
    logging.getLogger().setLevel(logging.INFO)
    sql_debug(True)
    yield
    logging.getLogger().setLevel(level)
    sql_debug(debug)

# @provider('log_all', scope='class', weight=-100, enabled=False)
# def log_all(Test):
#     return logging_context(Test)



# @with_cli_args
# @click.option('--log', 'scope', flag_value='test')
# @click.option('--log-all', 'scope', flag_value='all')
# def use_logging(scope):
#     if scope == 'test':
#         yield logging_context
#     elif scope =='all':
#         yield log_all




@provider()
class DBSessionProvider(object):

    __fixture__= 'db_session'

    weight = 30

    def __new__(cls, test):
        return db_session


@provider(__fixture__='rollback', weight=40)
@contextmanager
def do_rollback(test):
    try:
        yield
    finally:
        rollback()


@provider()
class SeparateProcess(object):

    # TODO read failures from sep process better

    __fixture__ = 'separate_process'

    enabled = False

    scope = 'class'

    def __init__(self, Test):
        self.Test = Test

    def __call__(self, func):
        def wrapper(Test):
            rnr = unittest.runner.TextTestRunner()
            TestCls = Test if isinstance(Test, type) else type(Test)
            def runTest(self):
                try:
                    func(Test)
                finally:
                    rnr.stream = unittest.runner._WritelnDecorator(StringIO())
            name = getattr(func, '__name__', 'runTest')
            Case = type(TestCls.__name__, (TestCls,), {name: runTest})
            Case.__module__ = TestCls.__module__
            case = Case(name)
            suite = unittest.suite.TestSuite([case])
            def run():
                result = rnr.run(suite)
                if not result.wasSuccessful():
                    sys.exit(1)
            p = Process(target=run, args=())
            p.start()
            p.join()
            case.assertEqual(p.exitcode, 0)
        return wrapper

    @classmethod
    def validate_chain(cls, fixtures, klass):
        for f in fixtures:
            if f.KEY in ('ipdb', 'ipdb_all'):
                return False
        for f in fixtures:
            if f.KEY == 'db' and f.PROVIDER in ('sqlserver', 'oracle'):
                return True

@provider()
class ClearTables(ContextDecorator):

    __fixture__ = 'clear_tables'

    def __init__(self, test):
        self.test = test

    def __enter__(self):
        pass

    @db_session
    def __exit__(self, *exc_info):
        db = self.test.db
        for entity in db.entities.values():
            if entity._database_.schema is None:
                break
            delete(i for i in entity)


@provider()
class NoJson1(ContextDecorator):

    __fixture__ = 'no_json1'

    def __init__(self, cls):
        self.Test = cls
        cls.no_json1 = True

    fixture_name = 'no_json1'

    def __enter__(self):
        self.json1_available = self.Test.db.provider.json1_available
        self.Test.db.provider.json1_available = False

    def __exit__(self, *exc_info):
        self.Test.db.provider.json1_available = self.json1_available

    scope = 'class'

    @classmethod
    def validate_chain(cls, fixtures, klass):
        for f in fixtures:
            if f.KEY in ('ipdb', 'ipdb_all'):
                return False
        for f in fixtures:
            if f.KEY == 'db' and f.PROVIDER in ('sqlserver', 'oracle'):
                return True


import signal

@provider()
class Timeout(object):

    __fixture__ = 'timeout'

    @with_cli_args
    @click.option('--timeout', type=int)
    def __init__(self, Test, timeout):
        self.Test = Test
        self.timeout = timeout if timeout else Test.TIMEOUT

    scope = 'class'
    enabled = False

    class Exception(Exception):
        pass

    class FailedInSubprocess(Exception):
        pass

    def __call__(self, func):
        def wrapper(test):
            p = Process(target=func, args=(test,))
            p.start()

            def on_expired():
                p.terminate()

            t = threading.Timer(self.timeout, on_expired)
            t.start()
            p.join()
            t.cancel()
            if p.exitcode == -signal.SIGTERM:
                raise self.Exception
            elif p.exitcode:
                raise self.FailedInSubprocess

        return wrapper

    @classmethod
    @with_cli_args
    @click.option('--timeout', type=int)
    def validate_chain(cls, fixtures, klass, timeout):
        if not getattr(klass, 'TIMEOUT', None) and not timeout:
            return False
        for f in fixtures:
            if f.KEY in ('ipdb', 'ipdb_all'):
                return False
        for f in fixtures:
            if f.KEY == 'db' and f.PROVIDER in ('sqlserver', 'oracle'):
                return True


pony_fixtures['test'].extend([
    'log',
    'clear_tables',
    'db_session',
])

pony_fixtures['class'].extend([
    'separate_process',
    'timeout',
    'db',
    'generate_mapping',
])

def db_is_present(providers, config):
    return providers

provider_validators.update({
    'db': db_is_present,
})

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com