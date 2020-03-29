---
title: pygtk example display rrd (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'display rrd'

Functions in program: 
* `def main():`

Modules used in program: 
* `import getpass`
* `import os`
* `import gtk`
* `import pygtk`
* `import gobject`

## python display rrd

Python pygtk example: display rrd

```python
#!/usr/bin/env python

import gobject
import pygtk
pygtk.require('2.0')
import gtk
import os
import getpass

class chip_exit:

    def create_window(self):
        self.window = gtk.Window()
        title = "Display image"
        self.window.set_title(title)
        self.window.set_border_width(5)
        self.window.set_size_request(502, 192)
        self.window.set_resizable(False)
        self.window.set_keep_above(True)
        self.window.stick
        self.window.set_position(1)
        self.window.connect("delete_event", gtk.main_quit)
        windowicon = self.window.render_icon(gtk.STOCK_QUIT, gtk.ICON_SIZE_MENU)
        self.window.set_icon(windowicon)

        # Create HBox for the image
        self.image = gtk.Image()
        self.image.set_from_file("temperatures.png")
        self.image.show()
        self.label_box = gtk.HBox()
        self.label_box.show()
        self.label_box.pack_start(self.image)

        # Create VBox and pack the HBox
        self.vbox = gtk.VBox()
        self.vbox.pack_start(self.label_box)
        self.vbox.show()

        self.window.add(self.vbox)
        self.window.show()

    def __init__(self):

        self.create_window()
        gobject.timeout_add(10000, self.timeout)

    def timeout(self):
        gtk.main_quit()

def main():
    gtk.main()

if __name__ == "__main__":
    go = chip_exit()
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
