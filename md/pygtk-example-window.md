---
title: pygtk example window (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'window'


Modules used in program: 
* `import os`
* `import gtk`
* `import pygtk`

## python window

Python pygtk example: window

```python
import pygtk
pygtk.require('2.0')
import gtk
import os


class Podcaster:

    wTree = None

    def __init__( self ):
        self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        self.window.connect("destroy", self.destroy)
        self.window.set_default_size(300,300)
        
        self.button = gtk.Button("Hello World")
        self.window.add(self.button)
        
        self.window.set_border_width(10)
        self.window.show_all()

    def main(self):
        gtk.main()

    def destroy(self, widget, data=None):
        gtk.main_quit()
        
if __name__ == "__main__":
    podcaster = Podcaster()
    podcaster.main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
