---
title: pygtk example gtkgst dialog (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'gtkgst dialog'


Modules used in program: 
* `import gtkgst`
* `import pygtk`

## python gtkgst dialog

Python pygtk example: gtkgst dialog

```python
try:
        import gtk
        import gobject
        from gtk import gdk
except:
        raise SystemExit

import pygtk
if gtk.pygtk_version < (2, 0):
        print("PyGtk 2.0 or later required for this widget")
        raise SystemExit

import gtkgst

class Dialog:
    def __init__(self):
        d = gtk.Dialog()
        d.add_buttons(gtk.STOCK_CANCEL, 1)
        camera = gtkgst.GtkGst("/dev/video0", [320, 240])
        d.vbox.pack_start(camera)
        capture = gtk.Button("Snap")
        self.image = None

        def capture_cb(*kwargs):
            self.image = camera.take_snapshot()
            get_code.set_sensitive(True)
            capture.set_sensitive(False)
            print("capture")

        capture.connect("clicked", capture_cb, None)
        
        d.vbox.pack_start(capture)

        d.show_all()
        ret = d.run()
        d.destroy()

if __name__=="__main__":
    d = Dialog()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
