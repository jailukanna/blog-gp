---
title: mysql example sql articles table (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'sql articles table'


Modules used in program: 
* `import mysql.connector`

## python sql articles table

Python mysql example: sql articles table

```python
import mysql.connector
all_sql_tables = [...]

# An example of a single table creation called "articles" in SQL command  
sql_articles = """CREATE TABLE IF NOT EXISTS articles (ID int AUTO_INCREMENT,
                                                     doi_link varchar(255) NOT NULL,
                                                     title varchar(255),
                                                     abstract TEXT,
                                                     publication_date varchar(255),
                                                     citations int,
                                                     UNIQUE (doi_link),  
                                                     PRIMARY KEY(ID));"""
all_sql_tables.append(sql_articles) # list of all sql tables creation

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
