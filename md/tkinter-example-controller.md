---
title: tkinter example controller (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'controller'


Modules used in program: 
* `import tkinter as Tk`

## python controller

Python tkinter example: controller

```python
import tkinter as Tk
from models import Model
from views import View


class Controller():
    def __init__(self):
        self.root = Tk.Tk()
        self.model = Model()
        # Pass to view links on root frame and controller object
        self.view = View(self.root, self)
        self.root.title("MVC example")
        self.root.geometry('300x200')
        # self.root.deiconify()
        self.root.mainloop()

    def add10(self, event):
        self.action(10)

    def add100(self, event):
        self.action(100)

    def action(self, addnum):
        # Pass data to view
        # self.view.viewPanel.entry.insert(0, self.model.do())
        num_input = self.view.viewPanel.v_entry.get()
        try:
            result = self.model.addnum(int(num_input), int(addnum))
        except ValueError:
            result = '¯\_(ツ)_/¯'
        self.view.viewPanel.v_num.set(result)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
