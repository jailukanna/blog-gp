---
title: tkinter example 02-button (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example '02-button'

Functions in program: 
* `def btn_call_back():`

Modules used in program: 
* `import tkinter as tk`

## python 02-button

Python tkinter example: 02-button

```python
#!/usr/bin/env python3
import tkinter as tk

def btn_call_back():
    print("Hello World.")

if __name__ == "__main__":
    root = tk.Tk()
    button = tk.Button(master=root,text="hello", command=btn_call_back)
    button.pack()
    root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
