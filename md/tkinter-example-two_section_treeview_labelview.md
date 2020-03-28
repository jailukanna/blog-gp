---
title: tkinter example two section treeview labelview (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'two section treeview labelview'


Modules used in program: 
* `import tkinter.scrolledtext as stxt`
* `import tkinter.filedialog as fdg`
* `import tkinter.messagebox as mbx`
* `import tkinter as tk`

## python two section treeview labelview

Python tkinter example: two section treeview labelview

```python
import tkinter as tk
from tkinter import ttk
import tkinter.messagebox as mbx
import tkinter.filedialog as fdg
import tkinter.scrolledtext as stxt

class MyWindow(tk.Tk):
    def __init__(self, screenName=None, baseName=None, className='Tk', useTk=1, sync=0, use=None,
                 geometry='600x400', title='My tkinter Window'):
        super().__init__(screenName, baseName, className, useTk, sync, use)
        self.geometry(geometry)
        self.title(title)
        MyFrame(self).pack(expand=True, fill=tk.BOTH)


class MyFrame(tk.Frame):
    def __init__(self, master=None, cnf={}, **kw):
        super().__init__(master, cnf, **kw)
        self.create_widgets()

    def create_widgets(self):
        # Define overall layout.
        tk.Grid.rowconfigure(self, 0, weight=1)
        tk.Grid.columnconfigure(self, 0, weight=1)
        tk.Grid.columnconfigure(self, 1, weight=4)

        # Add widgets.
        tree = ttk.Treeview(self)
        tree.grid(column=0, row=0, sticky=tk.NSEW)
        tree.insert('', 0, 'item1', text='first node', open=True, tag='one')
        tree.insert('', tk.END, 'item2', text='first subnode', tag='two')
        # tree.tag_configure('one', background='yellow')
        # tree.tag_configure('two', background='green')
        tree.tag_bind('one', '<1>', lambda e: display_area.config(text='one'))
        tree.tag_bind('two', '<1>', lambda e: display_area.config(text='two'))

        display_area = tk.Label(self, text='Welcome to the world of GUI programming.\nExciting journey begins.')
        display_area.grid(column=1, row=0, sticky=tk.NSEW)


if __name__ == '__main__':
    mw = MyWindow()
    mw.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
