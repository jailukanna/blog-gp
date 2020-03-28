---
title: tkinter example Lab%25201 MessageBox (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Lab%25201 MessageBox'

Functions in program: 
* `def Show(str):`

Modules used in program: 
* `import tkinter`

## python Lab%25201 MessageBox

Python tkinter example: Lab%25201 MessageBox

```python
#File name: MessageBox.py
import tkinter
from tkinter import *

def Show(str):
 root = Tk()
 root.title('Message Box')
 Label(root, justify=LEFT, text = str).grid(padx= 10, pady=5)

 root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
