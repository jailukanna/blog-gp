---
title: tkinter example simple messagebox (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'simple messagebox'


Modules used in program: 
* `import sys`

## python simple messagebox

Python tkinter example: simple messagebox

```python
import sys

if sys.version_info < (3,0):
        import Tkinter as tkinter
        import tkMessageBox as mbox
else:
        import tkinter
        import tkinter.messagebox as mbox

window = tkinter.Tk()
window.wm_withdraw()
mbox.showinfo('my app','my message')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
