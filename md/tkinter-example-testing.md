---
title: tkinter example testing (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'testing'

Functions in program: 
* `def on_click_2():`
* `def on_click_1():`

## python testing

Python tkinter example: testing

```python
try:
    import Tkinter as tkinter # for Python 2
except ImportError:
    import tkinter # for Python 3

def on_click_1():
    print("First handler fired")

def on_click_2():
    print("Second handler fired")

tk = tkinter.Tk()
myButton = tkinter.Button(tk, text="Click Me!")
myButton.pack()

# this first add is not required in this example, but it's good form.
myButton.bind("<Button>", on_click_1, add="+")

# this add IS required for on_click_1 to remain in the handler list
myButton.bind("<Button>", on_click_2, add="+")

tk.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
