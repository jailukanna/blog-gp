---
title: tkinter example Programming%2520herkansing%2520nim%2520spelletje (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Programming%2520herkansing%2520nim%2520spelletje'

Functions in program: 
* `def munt2(): #gedrag bij het pakken van 2 munten.`
* `def munt1(): #gedrag bij het pakken van 1 munt.`
* `def spel_gewonnen(): #winconditie maken.`
* `def volgendeSpeler(): #Om de zet van speler wisselen.`

Modules used in program: 
* `import tkinter.messagebox`

## python Programming%2520herkansing%2520nim%2520spelletje

Python tkinter example: Programming%2520herkansing%2520nim%2520spelletje

```python
#Alle benodigdheden importeren.
from tkinter import *
import tkinter.messagebox
from random import *

#tkinter scherm maken.
root = Tk()

#Startwaarden instellen.
aantal_munten = 7
winst = False
huidige_speler = str(randint(1, 2)) #Het spel start met een willekeurige speler.
munten = IntVar()
munten.set(aantal_munten)

def volgendeSpeler(): #Om de zet van speler wisselen.
    global huidige_speler
    if huidige_speler == '1':
        huidige_speler = '2'
    else:
        huidige_speler = '1'
    return huidige_speler

def spel_gewonnen(): #winconditie maken.
    global winst
    global aantal_munten
    if aantal_munten == 0:
        winst == True
    else:
        winst == False
    return winst

def munt1(): #gedrag bij het pakken van 1 munt.
    global munten
    if munten.get() > 0:
        munten.set(munten.get() - 1)
        a = "Er liggen " + str(munten.get()) + " munten op de stapel"
        print('a = {}'.format(a))
        text1.set(a)
        b = "speler " + str(volgendeSpeler()) + " is aan zet "
        text2.set(b)
        if munten.get() == 0:
            tkinter.messagebox.showinfo("WINNAAR", "Gefeliciteerd! Speler " + str(huidige_speler) +" heeft gewonnen")


def munt2(): #gedrag bij het pakken van 2 munten.
    global munten
    if munten.get() >= 2:
        munten.set(munten.get() - 2)
        a = "Er liggen " + str(munten.get()) + " munten op de stapel"
        print("a = {}".format(a))
        text1.set(a)
        b = "speler " + str(volgendeSpeler()) + " is aan zet "
        text2.set(b)
        if munten.get() == 0:
            tkinter.messagebox.showinfo("WINNAAR", "Speler " + str(huidige_speler) +" heeft gewonnen")
    else: #foutmelding wanneer je meer munten wilt pakken dan dat  er aanwezig zijn.
        tkinter.messagebox.showinfo("Hoe wil je dat doen?", "Er is nog maar 1 munt...")


#Tekst op scherm
a = "Er liggen " + str(munten.get()) + " munten op de stapel"
text1=StringVar()
text1.set(a)
muntenTekst = Label(root, textvariable = text1)
muntenTekst.pack(side = TOP)

#Tekst op scherm
b = "speler " + str(volgendeSpeler()) + " is aan zet "
text2=StringVar()
text2.set(b)
muntenTekst = Label(root, textvariable = text2)
muntenTekst.pack(side = TOP)

#gedrag van de knoppen bepalen
knopMunt1 = Button(root, text="Ik pak 1 munt", command=munt1)
knopMunt2 = Button(root, text="Ik pak 2 munten", command=munt2)
knopMunt1.pack(side=LEFT)
knopMunt2.pack(side=RIGHT)

#tkinter scherm levend houden
root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
