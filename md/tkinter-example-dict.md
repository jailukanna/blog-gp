---
title: tkinter example dict (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'dict'


Modules used in program: 
* `import time`
* `import os`

## python dict

Python tkinter example: dict

```python
#!/usr/bin/python
try:
    # Python2
    import Tkinter as tk
except ImportError:
    # Python3
    import tkinter as tk

import os
import time

 
 
root = tk.Tk()
# keep the window from showing
root.withdraw()
# read the clipboard
table=None
while True:
    try:
        get_clipb=root.selection_get(selection="CLIPBOARD")
        if table!=get_clipb:
            check_ok=False
            os.system("trans --speak :zh-TW \""+get_clipb+"\"")
            table=get_clipb
            #
             
    except:
        get_clipb=None
    time.sleep(0.5)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
