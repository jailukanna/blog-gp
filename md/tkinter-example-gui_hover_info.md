---
title: tkinter example gui hover info (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'gui hover info'


Modules used in program: 
* `import tkinter.ttk as ttk`

## python gui hover info

Python tkinter example: gui hover info

```python
from tkinter import *
import tkinter.ttk as ttk

pagination_style1 = {
    "button_spacing": 3,
    "button_padx":12,
    "button_pady": 6,
    "normal_button": {
        "font": ("Verdana", 10),
        "foreground": "#337ab7",
        "activeforeground":"#23527c",
        "background": "white",
        "activebackground": "#eee"
    },
    "selected_button": {
        "font":("Verdana", 10, "bold"),
        "foreground":"#fff",
        "activeforeground":"#fff",
        "background":"#337ab7",
        "activebackground":"#337ab7"
    }
}

root = Tk()

tree = ttk.Treeview(root)
tree.pack()

style = ttk.Style()
style.configure("mystyle.Treeview", highlightthickness=0, bd=0, font=('Calibri', 11)) # Modify the font of the body
style.configure("mystyle.Treeview.Heading", font=('Calibri', 13,'bold')) # Modify the font of the headings
style.layout("mystyle.Treeview", [('mystyle.Treeview.treearea', {'sticky': 'nswe'})]) # Remove the borders

tree["columns"] = ("one", "two", "three")
tree.column("one", width=150)
tree.column("two", width=150)
tree.column("three", width=150)
tree.heading("one", text="Naar")
tree.heading("two", text="Spoor")
tree.heading("three", text="Vetrektijd")
tree['show'] = 'headings'

root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
