---
title: mysql example selenium sql 2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'selenium sql 2'


Modules used in program: 
* `import mysql.connector`

## python selenium sql 2

Python mysql example: selenium sql 2

```python
df = pd.DataFrame(list(zip(Football_player, Team, goals, Assists, Shot_accuracy, Mins_per_goal)),
                  columns=['Football_player', 'Team', 'Goals', 'Assists', 'Shot_accuracy', 'Mins_per_goal'])

import mysql.connector
mydb = mysql.connector.connect(
    host='localhost',
    user='root',
    password='****',
    database='football_db'
)
# print(mydb)

 cursor = mydb.cursor()

 query = '''CREATE TABLE Top_players(
          Football_player text (255) NOT NULL,
          Team text (255) NOT NULL,
          Goals int(10) NOT NULL,
          Assists int(10) NOT NULL,
          Shot_accuracy int(10) NOT NULL,
          Mins_per_goal int(10) NOT NULL
          );
 '''

 cursor.execute(query)

df.to_sql(name='top_players', con=engine, index=False, if_exists='append')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
