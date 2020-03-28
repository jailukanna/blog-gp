---
title: tkinter example InputBoxTemplate (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'InputBoxTemplate'


Modules used in program: 
* `import tkinter.messagebox as msg`
* `import tkinter.simpledialog as dialog`

## python InputBoxTemplate

Python tkinter example: InputBoxTemplate

```python
# InputBoxTemplate.py
# by Chris Winikka
# A program template for getting user input with popup windows

# import statements
from tkinter import *
import tkinter.simpledialog as dialog
import tkinter.messagebox as msg

root = Tk()
w = Label(root, text="My Program")
w.pack()

# Welcome the user
msg.showinfo("Welcome", "Welcome to _______" )

# Get user info
name = dialog.askstring("Name", "What's your name?")
isAlive = msg.askyesno("Alive or Not", "Are you alive?")
age = dialog.askinteger("Age", "How old are you?")

##############################################################
# TODO: How do you get a float?
#       Ask a question where a precise number (floating point)
#       is required.
###############################################################

# Process information
output = "msg.askyesno() returns a {} value.".format(type(isAlive))
output += "\ndialog.askinteger() returns a {} value.".format(type(age))
output += "_______________ returns a ___ value"

# Output
msg.showinfo("Results", output)

# Clean up windows
root.destroy()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
