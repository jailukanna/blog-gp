---
title: tkinter example tk tags treeview test (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'tk tags treeview test'

Functions in program: 
* `def change_colors():`

Modules used in program: 
* `import tkinter.ttk as ttk`
* `import tkinter as tk`

## python tk tags treeview test

Python tkinter example: tk tags treeview test

```python
#This python3 testcase creates a ttk.Treeview
#with four items. Even rows have the 'blue' tag
#while odd rows have the 'red' tag
#Function change_colors exchange the tags every
#second
import tkinter as tk
import tkinter.ttk as ttk

root = tk.Tk()
tree = ttk.Treeview(root)

tree.insert("",0,text="Item #1", tags='blue')
tree.insert("",1,text="Item #2", tags='red')
tree.insert("",2,text="Item #3", tags='blue')
tree.insert("",3,text="Item #4", tags='red')

tree.tag_configure("blue",foreground="#00F")
tree.tag_configure("red",foreground="#F00")

tree.pack(fill=tk.BOTH, expand=1)

def change_colors():
    riids = tree.tag_has('red')
    biids = tree.tag_has('blue')
    #change red to blue
    for iid in riids:
        tree.item(iid,tags='blue')
    #change blue to red
    for iid in biids:
        tree.item(iid,tags='red')
    #relaunch after 1 sec
    root.after(1000,change_colors)

root.after(1000,change_colors)
root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
