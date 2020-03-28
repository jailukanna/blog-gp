---
title: tkinter example Lab%25201 lab1 4 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Lab%25201 lab1 4'


## python Lab%25201 lab1 4

Python tkinter example: Lab%25201 lab1 4

```python
s = "Results: \n"

# example 1
x = 5
if x > 3:
    s += "Correct \n"

# example 2
x = 15
if (x/3 > 6):
    s+= "Correct! \n"
else:
    s += "wrong\n"

# example 3
y = (0, 1, 2, 3, 4, 5)
for n in y:
    s += str(n) + " "
s += "\n"

# example 4
z = (0, 1, 2, 3, 4, 5)
for n in z:
    if n == 2:
        s += str(n * n) + " "
    else:
        s += str(n) + " "
s += "\n"

# example 5
w = 6; x = 5; y =3; z =7;
s += str(print(w, x, y, z)) + "\n"

# example 6
s += str(1 + 2 + 3 + 4 \
         + 5 +6 \
         + 7 + 8 + 9) + "\n"

# example 7
s += str (1 + 2 + 3
          + 4 + 5 + 6 +
          7 + 8 + 9) + "\n"

# example 8
s += str ([1 + 2 + 3 +
           4 + 5 + 6 +
           7 + 8 + 9]) + "\n"

# example 9
months = {'01':'January', '02':'February', '03':'March',\
          '04':'April', '05':'May', '06':'June', \
          '07':'July', '08':'August', '09':'September', \
          '10':'october', '11':'november', '12':'December'}
s += str(months)

# Message Box Code#######
from tkinter import *

root = Tk()
root.title('Message Box')
Label(root, justify=LEFT, text=s).grid(padx=10, pady=10)

root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
