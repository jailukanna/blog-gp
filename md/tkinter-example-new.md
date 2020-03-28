---
title: tkinter example new (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'new'


## python new

Python tkinter example: new

```python
try:
    # Python 3
    import tkinter as tk
    from tkinter import ttk
except ImportError:
    # Python 2
    import Tkinter as tk
    from Tkinter import ttk

class App(tk.Frame):
    def main(self):
        # get input value
        value = self.entryValue.get()

        # set value to label
        self.var.set(value)

    def __init__(self, *args, **kwargs):

        root = tk.Tk()
        # ---- Label ----
        self.var = tk.StringVar()
        self.var.set("Text will appear here.")
        myLabel = tk.Label(root, textvariable= self.var)
        myLabel.pack()

        # ---- Entry ----
        self.entryValue = tk.StringVar(root, value="")
        self.myEntry = ttk.Entry(root,
                                    textvariable=self.entryValue)
        self.myEntry.pack()

        # ---- Button ----
        myButton = ttk.Button(root, text="Show Text",
                                                command= lambda: self.main())
        myButton.pack()


        root.mainloop()

myInstance = App()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
