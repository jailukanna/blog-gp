---
title: mysql example testlink cases stats (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'testlink cases stats'

Functions in program: 
* `def tc_stats():`
* `def utf8(string):`

Modules used in program: 
* `import mysql.connector`
* `import csv`

## python testlink cases stats

Python mysql example: testlink cases stats

```python
#!/usr/bin/python

# -*- coding: utf-8 -*-

import csv

# The following module can be found here: http://dev.mysql.com/downloads/connector/python/
# or installed via aptitude (Debian/Ubuntu): aptitude install python-mysql.connector
import mysql.connector


EXEC_FACT_QUERY = (
    "SELECT "
    "nh1.parent_id AS case_id, "
    "ex.testplan_id, "
    "tcversions.tc_external_id, "
    "testprojects.prefix, "
    "nh2.name AS project_name "
    "FROM executions AS ex "
    "JOIN nodes_hierarchy AS nh1 ON ex.tcversion_id=nh1.id "
    "JOIN testplans ON ex.testplan_id=testplans.id "
    "JOIN tcversions ON ex.tcversion_id=tcversions.id "
    "JOIN testprojects ON testplans.testproject_id=testprojects.id "
    "JOIN nodes_hierarchy AS nh2 ON testprojects.id=nh2.id"
)

CASES_QUERY = (
    "SELECT id, name "
    "FROM nodes_hierarchy "
    "WHERE node_type_id=3"
)

def utf8(string):
    # database was in 'latin-1' before
    # so, we need to reencode in 'utf-8'
    return string.encode('utf-8')


class Case(object):

    @staticmethod
    def header():
        return ('id', 'name', 'exec_count', 'version_ids', 'projects',
                'smoke', 'sanity', 'regression', 'correction', 'functional')

    def __init__(self, id_, name):
        self.id = id_
        self.name = utf8(name)

        self.exec_count = 0

        self.is_smoke = False
        self.is_sanity = False
        self.is_functional = False
        self.is_correction = False
        self.is_regression = False

        self.projects = set()
        self.version_ids = set()


    def increase_counter(self):
        self.exec_count += 1

    def add_project(self, project_title):
        self.projects.add(utf8(project_title))

    def add_ver_id(self, id_):
        self.version_ids.add(id_)

    def get_row(self):
        versions = ', '.join(self.version_ids)
        projects = ', '.join(self.projects)
        return (self.id, self.name, self.exec_count, versions, projects,
                self.is_smoke, self.is_sanity, self.is_regression,
                self.is_correction, self.is_functional)


class Connector(object):
    def __init__(self, user, password, host, database):
        self.conn = mysql.connector.connect(user=user, password=password,
                                            host=host, database=database)
        self.cursor = self.conn.cursor()

    def __get_plan_ids(self, filtr):
        PLAN_QUERY = ('select tp.id, nh.name from nodes_hierarchy nh, testplans tp'
                 ' where nh.id=tp.id and nh.name like "%%%s%%";')
        self.cursor.execute(PLAN_QUERY % filtr)
        return [tpid for tpid, tpname in self.cursor]

    def get_sanity_plans_ids(self):
        return self.__get_plan_ids('sanity')

    def get_regr_plans_ids(self):
        return self.__get_plan_ids('regr')

    def get_corr_plans_ids(self):
        return self.__get_plan_ids('correction')

    def get_func_plans_ids(self):
        return self.__get_plan_ids('functional')

    def get_smoke_plans_ids(self):
        return self.__get_plan_ids('smoke')

    def get_cases_dict(self):
        self.cursor.execute(CASES_QUERY)
        return {i[0]:Case(*i) for i in self.cursor}

    def get_exec_facts(self):
        self.cursor.execute(EXEC_FACT_QUERY)
        return self.cursor.fetchall()

    def close(self):
        self.cursor.close()
        self.conn.close()


def tc_stats():
    conn = Connector('user', 'password', 'host', 'database')

    print('Getting data from DB...')

    cases_d = conn.get_cases_dict()

    # this wil work only if testplan's name/title
    # matches appropriate pattern defined in methods
    smoke_plans = conn.get_smoke_plans_ids()
    sanity_plans = conn.get_sanity_plans_ids()
    regr_plans = conn.get_regr_plans_ids()
    corr_plans = conn.get_corr_plans_ids()
    func_plans = conn.get_func_plans_ids()

    exec_facts = conn.get_exec_facts()

    conn.close()

    print('Counting metrics and comparison...')

    for case_id, plan_id, ver_ext_id, ver_prefix, project_name in exec_facts:
        case = cases_d[case_id]

        case.increase_counter()
        case.add_project(project_name)
        case.add_ver_id('-'.join((ver_prefix, str(ver_ext_id))))

        case.is_smoke = plan_id in smoke_plans
        case.is_sanity = plan_id in sanity_plans
        case.is_regression = plan_id in regr_plans
        case.is_correction = plan_id in corr_plans
        case.is_functional = plan_id in func_plans

    print('Get rid of cases not executed at least once')

    for id_, case in cases_d.copy().iteritems():
        if case.exec_count == 0:
            cases_d.pop(id_)

    print('Saving to CSV (in UTF-8, of course)')

    writer = csv.writer(open('testcases.csv', 'wb+'))
    writer.writerow(Case.header())

    for case in cases_d.values():
        writer.writerow(case.get_row())


if __name__ == '__main__':
    tc_stats()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
