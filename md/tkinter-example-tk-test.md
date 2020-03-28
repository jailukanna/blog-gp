---
title: tkinter example tk-test (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'tk-test'


## python tk-test

Python tkinter example: tk-test

```python
from __future__ import print_function
if str is bytes:
    try:
        import Tkinter as tk
    except:
        print("Cannot import Tkinter")
        exit(1)
else:
    try:
        import tkinter as tk
    except:
        print("Cannot impoer tkinter")
        exit(2)

try:
    root = tk.Tk()
    max_width = root.winfo_screenwidth() - 20
    max_height = root.winfo_screenheight() - 135
    max_size = (max_width, max_height)
    root.destroy()
    print("successful tkinter invocation")
    print("screen size:", max_size)
except:
    print("Cannot create tk window")
    exit(3)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
