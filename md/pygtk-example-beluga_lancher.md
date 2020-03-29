---
title: pygtk example beluga lancher (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'beluga lancher'

Functions in program: 
* `def tray_activate(event):`

Modules used in program: 
* `import os`
* `import gtk`
* `import pygtk`

## python beluga lancher

Python pygtk example: beluga lancher

```python
#!/usr/bin/python
#-*- coding:utf-8 -*-
import pygtk
import gtk
import os

def tray_activate(event):
    os.system("/opt/google/chrome/google-chrome --app=http://belugapods.com/site/mobile")

tray = gtk.StatusIcon()
tray.set_title("Beluga")
tray.set_from_file("~/Pictures/Beluga.png")
tray.connect('activate',  tray_activate)

gtk.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
