---
title: mysql example ISN-Module%2520MYSQL gestion frames frame principale (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ISN-Module%2520MYSQL gestion frames frame principale'

Functions in program: 
* `def frame_principal():`
* `def make_label(parent, img):`

Modules used in program: 
* `import os`

## python ISN-Module%2520MYSQL gestion frames frame principale

Python mysql example: ISN-Module%2520MYSQL gestion frames frame principale

```python
from tkinter import *
from Main import *
import os


def make_label(parent, img):
    label = Label(parent, image=img)
    label.pack()


def frame_principal():
    window = Tk()
    window.resizable(width=FALSE, height=FALSE)
    window.title("Brebie 2.0 - Fenêtre princial")

    l = LabelFrame(window, text="Module de jeu : ", width=1090, height=720)
    l.pack(fill="both", expand="yes")


    Frame_Principale_BackGround = os.path.join(script_dir, 'Frame_Principale_Background.png')
    Frame_Principale_BackGrounde = PhotoImage(file=Frame_Principale_BackGround)
    make_label(l,Frame_Principale_BackGrounde)


    window.mainloop()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
