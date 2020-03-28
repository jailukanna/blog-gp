---
title: tkinter example Lab%25201 lab1 5 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'Lab%25201 lab1 5'


Modules used in program: 
* `import MessageBox`
* `import InputBox`

## python Lab%25201 lab1 5

Python tkinter example: Lab%25201 lab1 5

```python
import InputBox
InputBox.ShowDialog("Enter the 1st Fahrenheit value:")
f = InputBox.GetInput()
c1 = (float(f) - 32) * (5/9)

InputBox.ShowDialog("Enter the 2nd Fahrenheit value:")
f = InputBox.GetInput()
c2 = (float(f) - 32) * (5/9)

import MessageBox
MessageBox.Show(str(c1) + "\n" + str(c2))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
