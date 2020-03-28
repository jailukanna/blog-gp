---
title: tkinter example tkinter template (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'tkinter template'


Modules used in program: 
* `import sys`

## python tkinter template

Python tkinter example: tkinter template

```python
"""Application Title

Description of the application.

"""


__author__ = "Name <email>"
__version__ = "0.0.0"


import sys
if sys.version[0] == '3':
    import tkinter as tk
    import tkinter.ttk as ttk
    from tkinter import scrolledtext
    from tkinter import filedialog
else:
    import Tkinter as tk
    import ttk as ttk
    import ScrolledText as scrolledtext
    import tkFileDialog as filedialog


class MainApplication(ttk.Frame):
    def __init__(self, parent, *args, **kwargs):
        ttk.Frame.__init__(self, parent, *args, **kwargs)
        self.parent = parent
        self.create_widgets()
        self.columnconfigure(0, weight=1)
        self.rowconfigure(0, weight=1)

    def create_widgets(self):
        """Contains all widgets in main application."""

        # Example widget
        b = ttk.Button(self, text="Hello")
        b.grid(column=0, row=0, sticky="nesw")

        # <create rest of GUI here>

        for child in self.winfo_children():
            child.grid_configure(padx=5, pady=5)


if __name__ == "__main__":
    root = tk.Tk()
    root.title(__doc__.strip())
    app = MainApplication(root, padding="5 5 5 5")
    app.pack(side="top", fill="both", expand=True)
    root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
