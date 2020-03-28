---
title: tkinter example input (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'input'

Functions in program: 
* `def main():`
* `def write_clipboard(string,selection='clipboard'):`

Modules used in program: 
* `import subprocess as sp`
* `import sys`

## python input

Python tkinter example: input

```python
import sys
import subprocess as sp

if sys.version_info.major == 2:
    import Tkinter as tk
    stringfy = lambda string: string
else:
    import tkinter as tk
    stringfy = lambda string: string.encode(encoding='utf-8')

class Gui(tk.Frame):
    DEFAULT_FONT = ('Ricty','20')
    def __init__(self, master):
        tk.Frame.__init__(self,master)
        self.pack()
        self.create_widgets()
        self.init_binds()
    
    def create_widgets(self):
        self.elem_text=tk.Text(self, font=self.DEFAULT_FONT)
        self.elem_text.pack({'side':'top','fill':tk.BOTH})
        self.elem_text.focus_set()
    
    def init_binds(self):
        self.master.bind('<Escape>',self.action_destroy)
        self.master.bind('<Control-Return>',self.action_finish)
    
    def action_destroy(self,*args):
        self.master.destroy()
    
    def action_finish(self,*args):
        text = self.elem_text.get('1.0',tk.END)
        write_clipboard(text)
        self.action_destroy()

def write_clipboard(string,selection='clipboard'):
    p = sp.Popen(['xclip','-selection',selection],stdin=sp.PIPE)
    p.communicate(stringfy(string))
    p.wait()

def main():
    w = tk.Tk()
    gui = Gui(w)
    gui.mainloop()


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
