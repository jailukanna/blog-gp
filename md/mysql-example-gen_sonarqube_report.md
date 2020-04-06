---
title: mysql example gen sonarqube report (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'gen sonarqube report'


Modules used in program: 
* `import mysql.connector`

## python gen sonarqube report

Python mysql example: gen sonarqube report

```python
import mysql.connector

config = {
    'user': 'root',
    'password': 'root',
    'host': '127.0.0.10',
    'db': 'sonar',
    'project': ''
}

cnx = mysql.connector.connect(user=config['user'], password=config['password'],
                              host=config['host'],
                              db=config['db'])
cursor = cnx.cursor()

query = """SELECT projects.long_name AS 'component name',
                ROUND(m3.value,1) AS 'ut-coverage',
                ROUND(m1.value,1) AS 'it-coverage',
                ROUND(m5.value,1) AS 'overall coverage',
                ROUND(m2.value) AS 'lines of code',
                ROUND(m4.value) AS 'open issues'
         FROM snapshots 
         JOIN projects ON snapshots.project_id=projects.id
         JOIN project_measures as m1 ON m1.snapshot_id=snapshots.id 
         JOIN project_measures as m2 ON m2.snapshot_id=snapshots.id
         JOIN project_measures AS m3 ON m3.snapshot_id=snapshots.id
         JOIN project_measures AS m4 ON m4.snapshot_id=snapshots.id
         JOIN project_measures AS m5 ON m5.snapshot_id=snapshots.id
         WHERE  snapshots.root_project_id=( SELECT p.id 
                                           FROM projects AS p 
                                           WHERE p.scope="PRJ" AND p.name LIKE "%1" LIMIT 0,1 ) AND
                snapshots.islast=1 AND
                snapshots.scope="PRJ" AND 
                m1.metric_id=59 AND 
                m2.metric_id=3 AND
                m3.metric_id=42 AND
                m4.metric_id=105 AND
                m5.metric_id=70;""" % (config['project'])

cursor.execute(query)

result = cursor.fetchall()

print(" name | ut | it | overall | lines | issues ")
for row in result:
    print(row[0] + " " + str(row[1]) + "% "  + str(row[2]) + "% " + str(row[3]) + "% " + str(row[4]))

#cursor.close()
cnx.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
