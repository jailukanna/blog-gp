---
title: mysql example generate mysql dict (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'generate mysql dict'


Modules used in program: 
* `import mysql.connector`
* `import sys`

## python generate mysql dict

Python mysql example: generate mysql dict

```python
import sys
import mysql.connector

if len(sys.argv) < 5:
    print("Usage: python3 generate_mysql_dict.py host dbname user password")
    sys.exit()

mydb = mysql.connector.connect(
    host=sys.argv[1],
    database=sys.argv[2],
    user=sys.argv[3],
    passwd=sys.argv[4]
)

database=sys.argv[2]

mycursor = mydb.cursor()
sql = 'show tables' 
mycursor.execute(sql)

myresult = mycursor.fetchall();
sqlList = [] 
for x in myresult:
    tmp = {}
    tmp["TABLE_NAME"] = x[0]
    sqlList.append(tmp)

for k, v in enumerate(sqlList):
    sql = """SELECT TABLE_COMMENT
    FROM INFORMATION_SCHEMA.TABLES
    WHERE table_name = '{1}' AND table_schema = '{0}'
""".format(database, v["TABLE_NAME"])

    mycursor.execute(sql)
    tableresult = mycursor.fetchall();
    for r in tableresult:
        sqlList[k]['TABLE_COMMENT'] = r[0]
        sqlList[k]['COLUMN'] = []

    sql = """SELECT COLUMN_NAME, COLUMN_TYPE, COLUMN_DEFAULT, IS_NULLABLE, EXTRA, COLUMN_COMMENT
    FROM INFORMATION_SCHEMA.COLUMNS
    WHERE table_name = '{1}' AND table_schema = '{0}'
""".format(database, v["TABLE_NAME"])

    mycursor.execute(sql)
    tableresult = mycursor.fetchall();
    for r in tableresult:
        sqlList[k]['COLUMN'].append(r)

output = ''; 
for k,v in enumerate(sqlList):
    output += """## 表名：{0} {1}
|字段名|数据类型|默认值|允许非空|自动递增|备注|
| ---- | ---- | ---- | ---- | ---- | ---- |
""".format(v['TABLE_NAME'], v['TABLE_COMMENT'])

    for f in v['COLUMN']:
        output += '|' + f[0]
        output += '|' + f[1]
        output += '|' + str(f[2])
        output += '|' + f[3]
        output += '|' + ('是' if f[4] == 'auto_increment' else ' ')
        output += '|' + f[5] + '|' + "\n";

print("""---
title: {0}
date: {1}
---
{2}
------
总共： {3}个数据表
""".format('数据字典'+database, database, output,str(len(sqlList))))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
