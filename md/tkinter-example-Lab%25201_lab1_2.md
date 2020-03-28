---
title: tkinter example Lab%25201 lab1 2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Lab%25201 lab1 2'


## python Lab%25201 lab1 2

Python tkinter example: Lab%25201 lab1 2

```python
s = "Results: \n"

# Example 1
s += "Hello world!\n"

# Example 2
s += "Hello" + "World" + "\n"

# Example 3
s += ("Hello"[-1]) + "\n"

# Example 4
fullname = "T Swift"
s += fullname + "\n"

# Example 5
course = "Python Programming"
s += course*2 + "\n"

# Example 6
s += str("apple" < "orang") + "\n"
s += str("tree" == "Tree") + "\n"
s += str("schools" > "school") + "\n"
s += str("Home" != "Home") + "\n"

# Example 7
s += ("\a\a\a\a\a") + "\n"

# Example 8
s += "\t\t\tWelcome to CIS247!\n"

###### Message Box Code #######
from tkinter import *

root = Tk()
root.title('Message Box')
Label(root, justify=LEFT, text=s).grid(padx=10, pady=10)

root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
