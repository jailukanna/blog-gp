---
title: tkinter example PathList (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'PathList'

Functions in program: 
* `def selectpath(): ##click Select path button`
* `def pathlistfrombutton():   #click List button`
* `def clearlist():  #click clear button`

Modules used in program: 
* `import tkinter.filedialog as filedialog`
* `import tkinter.messagebox as mbox`
* `import tkinter`
* `import os, sys`

## python PathList

Python tkinter example: PathList

```python
import os, sys
import tkinter
from tkinter import *
import tkinter.messagebox as mbox
import tkinter.filedialog as filedialog
files = ""

top = tkinter.Tk()
top.title("PathList 14/07/2014")
CheckVar1 = IntVar()

def clearlist():  #click clear button
   D.delete(1.0, END)


def pathlistfrombutton():   #click List button
   if len(G.get()) == 0:
      G.insert(0, "C:")
   files = os.listdir(G.get())
   if(CheckVar1.get()):
      files.sort()  #Sort 
   for f in files:
          D.insert(END, f)
          D.insert(END, "\n")

def selectpath(): ##click Select path button
   D.delete(1.0, END) 
   G.delete(0, END)
   directory = filedialog.askdirectory()
   G.insert(END, directory)


S = Scrollbar(top)
S.pack(side=RIGHT, fill=Y)
D = Text(top, height=50, width=100)
D.pack(side = BOTTOM)
S.config(command=D.yview)
D.config(yscrollcommand=S.set)


X = Label(top, text="                             ")
X.pack(side = LEFT)
H = tkinter.Button(top, text ="Select Path", command = selectpath)
H.pack(side = LEFT)
X = Label(top, text="   ")
X.pack(side = LEFT)
G = Entry(top, bd =5)
G.pack(side = LEFT)
X = Label(top, text="   ")
X.pack(side = LEFT)
F = tkinter.Button(top, text ="List", command = pathlistfrombutton)
F.pack(side = LEFT)
C1 = Checkbutton(top, text = "Sort", variable = CheckVar1, \
                 onvalue = 1, offvalue = 0, height=5, \
                 width = 20)
C1.pack(side = LEFT)
H = tkinter.Button(top, text ="Clear", command = clearlist)
H.pack(side = LEFT)


X = Label(top, text="\n\n\n\n\n")
X.pack()


top.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
