---
title: tkinter example errorMod (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'errorMod'

Functions in program: 
* `def displayError():`

Modules used in program: 
* `import tkinter`

## python errorMod

Python tkinter example: errorMod

```python
from tkinter import *
import tkinter

def displayError():
    """ Stores and displays error back to user"""
    notFound = 'Could Not locate file, please try again'
    tk = Tk()
    tk.title('Error(0x239ab)')
    displayMessage = tkinter.Label(tk, text=notFound)
    displayMessage.pack()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
