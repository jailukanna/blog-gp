---
title: mysql example db access (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'db access'


Modules used in program: 
* `import mysql.connector`

## python db access

Python mysql example: db access

```python
import mysql.connector

class Comment():

    def get_connection(self):
        conn = mysql.connector.connect(
            host = 'mysql',
            port = 3306,
            user = 'root',
            password = 'password',
            database = 'SAMPLE'
        )
        conn.ping(reconnect=True)

        return conn

    def insert(self, title, category, content):
        conn = self.get_connection()
        cur = conn.cursor()
        cur.execute('INSERT INTO COMMENT (TITLE, CATEGORY, CONTENT) VALUES (%s, %s, %s)', (title, category, content))

        cur.execute('select last_insert_id()')
        id = cur.fetchall()[0][0]
        
        cur.close()
        conn.commit()
        conn.close()

        return id

    def select(self, id):
        
        conn = self.get_connection()
        cur = conn.cursor()
        cur.execute('SELECT * FROM COMMENT WHERE ID=%s' ,(id,))
        sql_result = cur.fetchall()

        result = []

        for item in sql_result :
            index = item[0]
            title = item[1]
            category = item[2]
            content = item[3]

            result.append(dict(index=index, title=title, category=category, content=content))

        return result


    def update(self, id, title, category, content):

        conn = self.get_connection()
        cur = conn.cursor()

        cur.execute('UPDATE COMMENT SET TITLE=%s, CATEGORY=%s, CONTENT=%s WHERE ID=%s', (title, category, content, id, ))

        cur.close()
        conn.commit()
        conn.close()

        return id

    def delete(self, id):

        conn = self.get_connection()
        cur = conn.cursor()
        cur.execute('DELETE FROM COMMENT WHERE ID=%s', (id,))

        cur.close()
        conn.commit()
        conn.close()

        return id



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
