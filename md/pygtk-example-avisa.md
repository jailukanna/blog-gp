---
title: pygtk example avisa (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'avisa'


Modules used in program: 
* `import time`
* `import gtk`
* `import pygtk`

## python avisa

Python pygtk example: avisa

```python
#!/usr/bin/env python
# coding: utf-8

import pygtk
pygtk.require('2.0')
import gtk
import time

class Avisa(object):
    def __init__(self):
        self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        self.window.connect("delete_event", lambda *x: False)
        self.window.connect("destroy", lambda *x: gtk.main_quit())
        self.window.set_border_width(10)
        self.button = gtk.Button("Alongue agora!\n\n\n(clique para sair)")
        self.button.connect_object("clicked", gtk.Widget.destroy, self.window)
        self.window.add(self.button)
        self.button.show()
        self.window.show()
        self.window.fullscreen()
        gtk.main()

while True:
    h = Avisa()
    minutos = 40
    time.sleep(minutos*60)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
