---
title: tkinter example gui filter (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'gui filter'


Modules used in program: 
* `import tkinter.ttk as ttk`
* `import tkinter as tk`

## python gui filter

Python tkinter example: gui filter

```python
import tkinter as tk
import tkinter.ttk as ttk


class Filter(tk.Frame):
    def __init__(self, parent):
        super().__init__(parent)
        self.entry_box = tk.Entry(self)

    def validate(self):
        pass


class FilterCriteria(ttk.Combobox):
    _CRITERIA = ['Is equal to', 'Is not equal to', 'Starts with', 'Does not contain', 'Ends with']

    def __init__(self, parent):
        ttk.Combobox.__init__(self, parent, values=FilterCriteria._CRITERIA, state="readonly")
        # self.pack(anchor=tk.CENTER)




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
