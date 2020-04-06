---
title: mysql example maria db client (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'maria db client'

Functions in program: 
* `def main():`
* `def insert_channels(mariadb_connection, number_of_channels, levels):`

Modules used in program: 
* `import mysql.connector as mariadb`

## python maria db client

Python mysql example: maria db client

```python
#!/usr/bin/python
import mysql.connector as mariadb

def insert_channels(mariadb_connection, number_of_channels, levels):
    cursor = mariadb_connection.cursor()

    for root in range(10, 16):
        cursor.execute('INSERT INTO channel values ({}, NULL, 1, 0, NULL)'.format(root))
        for id1 in range(root*10, root*10+10):
            cursor.execute('INSERT INTO channel values ({}, {}, 1, 0, NULL)'.format(id1, root))
            for id2 in range((root+id1)*100, (root+id1)*100+10):
                cursor.execute('INSERT INTO channel values ({}, {}, 1, 0, NULL)'.format(id2, id1))
                for id3 in range((root+id1+id2)*1000, (root+id1+id2)*1000+10):    
                    cursor.execute('INSERT INTO channel values ({}, {}, 1, 0, NULL)'.format(id3, id2))
    mariadb_connection.commit()

    cursor.execute("SELECT id, channel_id, video_manager_id FROM channel WHERE video_manager_id=1")
    for (id, channel_id, video_manager_id) in cursor:
        print("id: {}, channel_id: {}, video_manager_id: {}").format(id, channel_id, video_manager_id)

def main():
    mariadb_connection = mariadb.connect(host='192.168.99.100', user='root', password='admin', database='vam')        
    insert_channels(mariadb_connection, 100, 5)

if __name__ == "__main__":
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
