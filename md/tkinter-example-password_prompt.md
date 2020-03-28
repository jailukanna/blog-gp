---
title: tkinter example password prompt (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'password prompt'


Modules used in program: 
* `import Tkinter, tkSimpleDialog `

## python password prompt

Python tkinter example: password prompt

```python
# stolen from stackoverflow, based on how james and i share scripts:

import Tkinter, tkSimpleDialog 
root = Tkinter.Tk() # dialog needs a root window, or will create an "ugly" one for you
root.withdraw() # hide the root window
password = tkSimpleDialog.askstring("Password", "Enter password:", show='*', parent=root)
root.destroy() # clean up after yourself!

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
