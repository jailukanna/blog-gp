---
title: tkinter example untitled6 parent (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'untitled6 parent'


Modules used in program: 
* `import mdc`
* `import tkinter as tk`

## python untitled6 parent

Python tkinter example: untitled6 parent

```python
import tkinter as tk
import mdc

class SampleApp(tk.Tk):
    def __init__(self):
        tk.Tk.__init__(self)
        self.entrya = tk.Entry(self)
        self.entryb = tk.Entry(self)
        self.button = tk.Button(self, text="Get", command=self.on_button)
        self.button.pack()
        self.entrya.pack()
        self.entryb.pack()

    def on_button(self):
        a=int(str(self.entrya.get()))
        b=int(str(self.entryb.get()))
        mdcab=mdc.mdc_r(a,b)
        print(mdcab)

w = SampleApp()
w.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
