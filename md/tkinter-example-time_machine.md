---
title: tkinter example time machine (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'time machine'

Functions in program: 
* `def TimeMachineACTIVATOR():`

Modules used in program: 
* `import time`

## python time machine

Python tkinter example: time machine

```python
try:
    import Tkinter  as tkinter
    import tkFont
except ImportError:
    # python 3
    import tkinter
    import tkinter.font as tkFont

import time

root = tkinter.Tk()
root.overrideredirect(1)

def TimeMachineACTIVATOR():
    window.config(text=time.strftime("%I:%M:%S"))
    window.after(1000, TimeMachineACTIVATOR)

font = tkFont.Font(family="Helvetica", size=16)
window = tkinter.Button(root,
                        text = "THE TIME MACHINE",
                        font=font,
                        command=lambda: root.destroy())
window.pack(fill="both")

root.update()

width, height = root.winfo_width(), root.winfo_height()
x = (root.winfo_screenwidth() / 2) - (width / 2)
y = (root.winfo_screenheight() / 2) - (height / 2)
root.geometry("%ix%i+%i+%i" % (width, height, x, y))

TimeMachineACTIVATOR()

root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
