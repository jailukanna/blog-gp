---
title: tkinter example irctcApp (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'irctcApp'


## python irctcApp

Python tkinter example: irctcApp

```python
try:
    import tkinter
except ImportError:
    import Tkinter as tkinter

mainWindow = tkinter.Tk()

mainWindow.title("  New IRCTC APP")
mainWindow.geometry('640x480+8+400')

label = tkinter.Label(mainWindow, text="Please SignUp or Login")
label.pack(side='top')



button1 = tkinter.Button(mainWindow, text="Login")
button1.pack(side='left', anchor='w')
button1.o
button2 = tkinter.Button(mainWindow, text="Register")
button2.pack(side='right', anchor='e')

mainWindow.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
