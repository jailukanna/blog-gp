---
title: tkinter example 01.06.19 calc1 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example '01.06.19 calc1'

Functions in program: 
* `def division():`
* `def multiply():`
* `def subtract():`
* `def addition():`

Modules used in program: 
* `import tkinter as tk`

## python 01.06.19 calc1

Python tkinter example: 01.06.19 calc1

```python
import tkinter as tk
root = tk.Tk()


def addition():
	a = int(entry1.get())
	b = int(entry2.get())
	example_button.config(text=(a, "+", b, "=", (a+b)))


def subtract():
	a = int(entry1.get())
	b = int(entry2.get())
	example_button.config(text=(a, "-", b, "=", (a-b)))


def multiply():
	a = int(entry1.get())
	b = int(entry2.get())
	example_button.config(text=(a, "*", b, "=", (a*b)))


def division():
	a = int(entry1.get())
	b = int(entry2.get())
	example_button.config(text=(a, "/", b, "=", (a/b)))


entry1 = tk.Entry(width="10")
entry2 = tk.Entry(width="10")
addition_button = tk.Button(text="+", width="3", command=addition)
subtract_button = tk.Button(text="-", width="3", command=subtract)
multiply_button = tk.Button(text="*", width="3", command=multiply)
division_button = tk.Button(text="/", width="3", command=division)
example_button = tk.Label(width="10")

entry1.pack()
entry2.pack()
addition_button.pack()
subtract_button.pack()
multiply_button.pack()
division_button.pack()
example_button.pack()

root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
