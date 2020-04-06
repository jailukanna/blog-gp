---
title: mysql example ISN-Module%2520MYSQL gestion frames gestion frame inscription (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ISN-Module%2520MYSQL gestion frames gestion frame inscription'

Functions in program: 
* `def frame_inscription():`

## python ISN-Module%2520MYSQL gestion frames gestion frame inscription

Python mysql example: ISN-Module%2520MYSQL gestion frames gestion frame inscription

```python
from tkinter import *
from Sqllib.inscription import insert_players
from Main import *

identifiant_joueur = "e";


def frame_inscription():
    # ---------------- Définition de la zone de travail ------------------------
    window = Tk()
    window.resizable(width=FALSE, height=FALSE)
    window.title("Brebie 2.0 module d'inscription")

    # -----------Définition de toutes les textes Boxes de la frame de connexion -------------------------------

    l = LabelFrame(window, text="Module de connexion : ", width=480, height=320)
    l.pack(fill="both", expand="yes")

    brebie_is = Label(l, text="Module d'inscription")
    identifiant = Label(l, text="Identifiant : ")
    mot_de_passe = Label(l, text="Mot de passe : ")

    utilisateur = Entry(window)
    mdp = Entry(window, show='*')

    def inscription(event):
        user = utilisateur.get().strip()
        passwd = mdp.get().strip()
        insert_players(user, passwd)
        lancerconfig = True;


    utilisateur.insert(0, "Insérez-votre Login")
    mdp.insert(0, "mdppardefault")
    window.bind('<Return>', inscription)

    inscription = Button(window, text="Inscription", command=inscription)
    inscription.place(x=250, y=154, width=95)

    # ----------- Géometrie de la frame  -------------------------------

    brebie_is.pack()
    brebie_is.place(x=150, y=13)

    identifiant.pack()
    identifiant.place(x=5, y=127)

    mot_de_passe.pack()
    mot_de_passe.place(x=5, y=155)

    utilisateur.pack()
    utilisateur.place(x=100, y=145)

    mdp.pack()
    mdp.place(x=100, y=173)

    window.mainloop()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
