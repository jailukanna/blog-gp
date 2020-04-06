---
title: mysql example ISN-Module%2520MYSQL Sqllib getinfo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ISN-Module%2520MYSQL Sqllib getinfo'

Functions in program: 
* `def verifid(nom, mdpe):`
* `def get_brebie_lvl(nom,brebie):`
* `def verifexist(nom):`
* `def getmoney(nom):`
* `def getlvl(nom):`

## python ISN-Module%2520MYSQL Sqllib getinfo

Python mysql example: ISN-Module%2520MYSQL Sqllib getinfo

```python
from Sqllib.sql import consql

def getlvl(nom):
  queryy = ('SELECT `lvl` FROM `players` WHERE name = "'+nom+'"')
  lvl = consql(queryy)
  return int(lvl)

def getmoney(nom):
  queryy = ('SELECT `money` FROM `players` WHERE name = "'+nom+'"')
  money = consql(queryy)
  return int(money)

def verifexist(nom):
    queryy = ('SELECT `name` FROM `players` WHERE name = "' + nom + '"')
    exist = consql(queryy)
    if exist is False:
        return False
    else:
        return True

def get_brebie_lvl(nom,brebie):
  queryy = ('SELECT `lvl_brebie_'+str(brebie)+'` FROM `brebie_effectif` WHERE player_id = "'+nom+'"')
  lvl = consql(queryy)
  return int(lvl)


def verifid(nom, mdpe):
  queryy = ('SELECT `passwd` FROM `players` WHERE name = "'+nom+'"')
  mdp = consql(queryy)
  if str(mdp) == mdpe:
      print("Bienvenue " + nom + " !")
      print("Vous êtes niveau : " + str(getlvl(nom)))
      print("Vous avez " + str(getmoney(nom)) + " piéces d'or !")
      return True;
  else:
      print("Nous n'avons pas pu vérifier votre identité, Réassyez plus tard !")
      return False;





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
