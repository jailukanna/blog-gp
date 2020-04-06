---
title: mysql example connexionbd (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'connexionbd'

Functions in program: 
* `def create_connection(fichier):`

Modules used in program: 
* `import mysql`

## python connexionbd

Python mysql example: connexionbd

```python
# Avec l'aide du professeur Georges Debay

import mysql
from mysql import Error


def create_connection(fichier):
    try:
        conn = mysql.connect(fichier)
        print(mysql.version)
    except Error as e:  # Peut nommer e ou autre
        print(e)
    finally:
        conn.close()
        # Dans tous les cas, fermer la connexion



create_connection("toto.db") #Nommer la bd

#Installer MySQL au préalable (open source). 
#Peut aussi faire appel à SQLite (la bd comprise dans Pycharm), avec le même procédé.

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
