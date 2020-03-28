---
title: tkinter example label (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'label'


Modules used in program: 
* `import tkinter as tk`

## python label

Python tkinter example: label

```python
import tkinter as tk

win = tk.Tk()
win.title("Label")
win.minsize(200, 50)

# create label without variable
tk.Label(win, text="D.Raccoon - 1").pack()

# create label with variable
label = tk.Label(win, text="D.Raccoon - 2")
label.pack()

win.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
