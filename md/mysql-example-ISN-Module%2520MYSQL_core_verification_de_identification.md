---
title: mysql example ISN-Module%2520MYSQL core verification de identification (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ISN-Module%2520MYSQL core verification de identification'

Functions in program: 
* `def verifplayers(name_of_players, passwd_of_players):`
* `def verifco():`

Modules used in program: 
* `import socket`

## python ISN-Module%2520MYSQL core verification de identification

Python mysql example: ISN-Module%2520MYSQL core verification de identification

```python
from Sqllib.getinfo import verifid
from Sqllib.getinfo import verifexist
import socket

def verifco():
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.connect(("www.google.com", 80))
        return True
    except socket.gaierror:
        return False


def verifplayers(name_of_players, passwd_of_players):

    connexion = verifco()
    if connexion is True:
     print("Le nom du joueur est : "+name_of_players)
     reel = verifexist(name_of_players)
     if reel is True:
         print("Le mdp reçu est le suivant : " + str(passwd_of_players))
         print("Le joueur existe c'est parti pour la verif ID !")
         verifid(str(name_of_players), str(passwd_of_players))
         return True;
     else:
         print("Vous n'êtes pas inscrit sur notre Base de Donnée !, Inscrivez-Vous !!!")
         return False;

    else:
        print("Upsss, Il semblerait que vous n'êtes pas connecté à internet !!!")
        return False;


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
