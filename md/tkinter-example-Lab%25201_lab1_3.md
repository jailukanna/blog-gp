---
title: tkinter example Lab%25201 lab1 3 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Lab%25201 lab1 3'


## python Lab%25201 lab1 3

Python tkinter example: Lab%25201 lab1 3

```python
s = "Results: \n"

# example 1
unit_price = 5.75
qty = 12
s += "$" + str (unit_price * qty) + "\n"

# example 2
x = 1
x = x + 2
x = x + 3
x = x + 4
x = x + 5

s += str(x) + "\n"

# example 3
s+= "Python" + "3" + "" + "programming\n"

# example 4
s += "" "Python" "3" " " "programming\n"

# example 5
x=3.14
s+= str (int(x)) + "\n"

# example 6
y = "0.5832"
s += str(float(y)) + "\n"

####### Message Box Code#######
from tkinter import *
root = Tk()
root.title('Message Box')
Label (root, justify=LEFT, text=s).grid(padx=10, pady=10)

root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
