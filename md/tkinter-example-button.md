---
title: tkinter example button (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'button'

Functions in program: 
* `def click_action():`

Modules used in program: 
* `import tkinter as tk`

## python button

Python tkinter example: button

```python
import tkinter as tk

# after button clicked, change config
def click_action():
    # change text color and message for label 
    tk_label.configure(fg="yellow")
    tk_label.configure(text="After")

    # change text and disable the button
    tk_button.configure(text="disabled")
    tk_button.configure(state="disabled")

win = tk.Tk()
win.title("Button")
win.minsize(200, 50)

# create label to show state
tk_label = tk.Label(win, text="Before")
tk_label.pack()

# create button to do action
tk_button = tk.Button(win, text="disable", command=click_action)
tk_button.pack()

win.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
