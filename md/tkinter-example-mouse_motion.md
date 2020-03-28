---
title: tkinter example mouse motion (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'mouse motion'


Modules used in program: 
* `import sys`

## python mouse motion

Python tkinter example: mouse motion

```python
#!/usr/bin/python
#
# Ben Cohen, October 2017.
#
# mouse_motion.py: Display "breadcrumb trail" to test mouse motion events.
#
# Move the mouse and the positions of the motion events will be displayed.
# Clear the canvas by pressing Backspace.  Quit by pressing Escape.

import sys
if sys.version_info[0] == 2:
    import Tkinter as tk
else:
    import tkinter as tk

class MouseMotionTest(object):
    def __init__(self, master, **kwargs):
        self.master = master
        master.attributes("-zoomed", True)  # or could use "-fullscreen"
        master.title("Mouse motion test")
        master.bind('<Escape>',self.quit)            
        master.bind('<BackSpace>',self.clear)            
        canvas = tk.Canvas(master)
        canvas.pack(expand = tk.YES, fill = tk.BOTH)
        canvas.bind('<Motion>',self.mousemoved)            
        canvas.config(cursor = "top_left_arrow")
        self.canvas = canvas

    def quit(self, event):
        self.master.quit()

    def clear(self, event):
        self.canvas.delete("all")

    def mousemoved(self, event):
        x = event.x
        y = event.y
        self.canvas.create_line(x - 3, y, x + 3, y, fill = "#000000")
        self.canvas.create_line(x, y - 3, x, y + 3, fill = "#000000")

root=tk.Tk()
app=MouseMotionTest(root)
root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
