---
title: tkinter example most userful (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'most userful'

Functions in program: 
* `def message_and_close():`

Modules used in program: 
* `import tkinter.messagebox`
* `import tkinter as tk`

## python most userful

Python tkinter example: most userful

```python
import tkinter as tk
import tkinter.messagebox

def message_and_close():
    res = tk.messagebox.askquestion("Close program", "Are you sure?", icon='warning')
    if res == "yes":
        root.destroy()

root = tk.Tk()
button_frame = tk.Frame(root)
button_frame.pack()
reset_button = tk.Button(button_frame, text="Close", command=message_and_close)
reset_button.grid()

root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
