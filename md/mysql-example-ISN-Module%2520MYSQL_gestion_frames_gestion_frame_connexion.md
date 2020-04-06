---
title: mysql example ISN-Module%2520MYSQL gestion frames gestion frame connexion (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ISN-Module%2520MYSQL gestion frames gestion frame connexion'

Functions in program: 
* `def fenndeco():`

## python ISN-Module%2520MYSQL gestion frames gestion frame connexion

Python mysql example: ISN-Module%2520MYSQL gestion frames gestion frame connexion

```python
from gestion_frames.gestion_frame_inscription import frame_inscription
from gestion_frames.frame_principale import *
from core.verification_de_identification import verifplayers
from core.config import *
from tkinter import *




def fenndeco():
    # -----------Définition de la fenêtre et de la zone de travail (FRAME) -------------------------------
    window = Tk()
    window.resizable(width=FALSE, height=FALSE)
    window.title("Brebie 2.0")
    identifiant_joueur = "e";

    # -----------Définition de toutes les textes Boxes de la frame de connexion -------------------------------

    l = LabelFrame(window, text="Module de connexion : ", width=480, height=320)
    l.pack(fill="both", expand="yes")

    brebie_bv = Label(l, text="Bienvenue dans le jeu Brebie 2.0")
    message_de_connexion = Label(l, text="Connectez-Vous où bien rejoingez-nous !!")
    identifiant = Label(l, text="Identifiant : ")
    mot_de_passe = Label(l, text="Mot de passe : ")

    # -----------Définition fct boutton + Boutton -------------------------------

    def connect():
        user = utilisateur.get().strip()
        passwd = mdp.get().strip()
        if verifplayers(user, passwd) is True:
            id_player = utilisateur.get().strip();
            update_config_file(user)
            window.destroy()
            frame_principal()

        else:
            print("Vérifier vos informations de connexion !")



    def lancer_inscription():
        window.destroy()
        frame_inscription()

    inscription = Button(window, text="Inscrivez-vous", command=lancer_inscription)
    inscription.place(x=240, y=240, width=95)
    connexion = Button(window, text="Connexion", command=connect)
    connexion.place(x=135, y=240, width=95)

    # ----------- Géometrie de la frame  -------------------------------

    utilisateur = Entry(window)
    mdp = Entry(window, show='*')

    utilisateur.insert(0, "Insérez-votre Login")
    mdp.insert(0, "mdppardefault")

    brebie_bv.pack()
    brebie_bv.place(x=150, y=13)

    message_de_connexion.pack()
    message_de_connexion.place(x=129, y=31.5)

    identifiant.pack()
    identifiant.place(x=125, y=109)

    mot_de_passe.pack()
    mot_de_passe.place(x=125, y=137)

    utilisateur.pack()
    utilisateur.place(x=215, y=127)

    mdp.pack()
    mdp.place(x=215, y=155)

    window.mainloop()






```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
