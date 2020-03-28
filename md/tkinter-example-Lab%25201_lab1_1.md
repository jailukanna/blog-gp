---
title: tkinter example Lab%25201 lab1 1 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Lab%25201 lab1 1'


## python Lab%25201 lab1 1

Python tkinter example: Lab%25201 lab1 1

```python
s = "Results:\n"  # declare s string

# example 1
x = 21
s += str(x) + "\n"  # + concateneates

# example 2
x = 12**2
s += str(x/2) + "\n"

# example 3
y = 4.35
s += str(y*2) + "\n"

# example 4
s += str(5.231 * 6.172 * 29) + "\n"

# example 5
s += str(2 + 6) + "\n"

# example 6
s += str((3 + 1j) * (3 - 1j)) + "\n"

# example 7
s += str(22/  7) + "\n"

###### Message Box Code ######
from tkinter import *
root = Tk()
root.title('Message Box')
Label(root, justify=LEFT, text=s).grid(padx=10, pady=10)

root.mainloop()






```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
