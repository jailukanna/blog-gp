---
title: tkinter example text box (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'text box'

Functions in program: 
* `def click_action():`

Modules used in program: 
* `import tkinter as tk`

## python text box

Python tkinter example: text box

```python
import tkinter as tk

# set label with user input
def click_action():
    nameLabel.configure(text=name.get())

win = tk.Tk()
win.title("Text Box")

fieldLabel = tk.Label(win, text="Name:")
fieldLabel.grid(row=0, column=0)

nameLabel = tk.Label(win, text="NONE")
nameLabel.grid(row=0, column=1)

# create string variable to receive user input
name = tk.StringVar()

# create text box
entry = tk.Entry(win, textvariable=name, bg="white", fg="black")
entry.grid(row=1, column=0)

# set text box as the active widget
entry.focus()

button = tk.Button(win, text="Enter", command=click_action)
button.grid(row=1, column=1)

win.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
