---
title: pygtk example geel (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'geel'


Modules used in program: 
* `import gtk`
* `import pygtk`

## python geel

Python pygtk example: geel

```python
import pygtk
pygtk.require('2.0')
import gtk

class FirstWin:
   def __init__(self):
       self.win = gtk.Window(gtk.WINDOW_TOPLEVEL)
       color = gtk.gdk.color_parse('#6495ED')
       self.win.modify_bg(gtk.STATE_NORMAL, color)
       #  To change the color again, just modify it again
       self.win.show()

   def main(self):
       gtk.main()

if __name__ == "__main__":
   first = FirstWin()
   first.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
