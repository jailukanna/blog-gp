---
title: mysql example ISN-Module%2520MYSQL Sqllib sql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ISN-Module%2520MYSQL Sqllib sql'

Functions in program: 
* `def insertplayer(query):`
* `def consql(query):`

Modules used in program: 
* `import mysql.connector`

## python ISN-Module%2520MYSQL Sqllib sql

Python mysql example: ISN-Module%2520MYSQL Sqllib sql

```python
import mysql.connector
from mysql.connector import errorcode

config = {
  'user': 'brebie_bd',
  'password': 'Azerty1945',
  'host': '41.230.198.0',
  'database': 'brebie_bd',
  'raise_on_warnings': True
}

def consql(query):
  try:
    cnx = mysql.connector.connect(**config)
  except mysql.connector.Error as err:
    if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
      print("username ou mdp eronnee")
      return False
    elif err.errno == errorcode.ER_BAD_DB_ERROR:
      print("La base de donnée n'existe pas !!!")
    else:
      print(err)
  else:
    curseur = cnx.cursor(buffered=True)
    curseur.execute(query)
    value = curseur.fetchone()
    cnx.close()
    curseur.close()
    if not(value):
        return False
    else:
     return value[0]

def insertplayer(query):
  print("Insertion du joueur en cours !")
  try:
    cnx = mysql.connector.connect(**config)
    print("Connexion effectué")
  except mysql.connector.Error as err:
    if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
      print("username ou mdp eronnee")
      return False
    elif err.errno == errorcode.ER_BAD_DB_ERROR:
      print("La base de donnée n'existe pas !!!")
    else:
      print(err)
  else:
    curseur = cnx.cursor()
    curseur.execute(query)
    cnx.commit()
    cnx.close()
    curseur.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
