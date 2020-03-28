---
title: tkinter example trbutton (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'trbutton'

Functions in program: 
* `def callback(e):`

## python trbutton

Python tkinter example: trbutton

```python
from tkinter import *



def callback(e):
    print('Нажата кнопка', e.widget['text'])

root = Tk()
button1 = Button(root, text='1')
button1.pack()
button2 = Button(root, text='2')
button2.pack()
root.bind_class('Button', '<1>', callback)
root.mainloop()

"""

button when pressed shows in the terminal which button you pressed

"""


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
