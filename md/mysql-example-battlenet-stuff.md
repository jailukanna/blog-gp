---
title: mysql example battlenet-stuff (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'battlenet-stuff'

Functions in program: 
* `def main():`
* `def get_rarest_items(mycursor):`

Modules used in program: 
* `import mysql.connector`

## python battlenet-stuff

Python mysql example: battlenet-stuff

```python
# This is a python version of https://newswire.theunderminejournal.com/sample2.php

# requirements
#    pip install mysql-connector

import mysql.connector
from textwrap import dedent


def get_rarest_items(mycursor):
    region = 'US'
    slug = 'medivh'

    sql = dedent("""\
    SELECT i.id, i.name_enus, s.lastseen
    FROM tblItemSummary s
    JOIN tblDBCItem i ON i.id = s.item
    JOIN tblRealm r ON s.house = r.house
    WHERE r.region = %s
        AND r.slug = %s
        AND s.level = if(i.class IN (2,4), i.level, 0)
        AND i.quality > 1
    ORDER BY s.lastseen ASC
    LIMIT 20
    """)
    data = [region, slug]

    mycursor.execute(sql, data)
    for x in mycursor:
        item_id = x[0]
        item_name = x[1]
        last_seen = x[2]
        print(f'[{item_id}] {item_name} was last seen {last_seen}')


def main():
    mydb = mysql.connector.connect(
        host="newswire.theunderminejournal.com",
        user="",
        passwd="",
        database="newsstand"
    )

    mycursor = mydb.cursor()
    get_rarest_items(mycursor)


if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
