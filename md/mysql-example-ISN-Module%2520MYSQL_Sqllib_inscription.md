---
title: mysql example ISN-Module%2520MYSQL Sqllib inscription (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ISN-Module%2520MYSQL Sqllib inscription'

Functions in program: 
* `def insert_players(id,mdp):`

## python ISN-Module%2520MYSQL Sqllib inscription

Python mysql example: ISN-Module%2520MYSQL Sqllib inscription

```python
from Sqllib.sql import insertplayer
from core.verification_de_identification import *

def insert_players(id,mdp):
    exist = verifexist(id)
    if exist is True:
        print("Le joueur existe, il veut tuer la BDD")
    else:
        print("Le joueur inserez s'appelle " + id + ". Le mdp choisi est le suivant : " + mdp)
        query = (
                    "INSERT INTO `players` (`name`, `passwd`, `money`, `lvl`) VALUES ('" + id + "', '" + mdp + "', '0', '0');")
        query = (
                    "INSERT INTO `brebie_effectif` (`player_id`, `lvl_brebie_1`, `lvl_brebie_2`, `lvl_brebie_3`, `lvl_brebie_4`) VALUES ('"+id+"', '1', '1', '1', '1');")
        print("Querryy : " + query)
        insertplayer(query)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
