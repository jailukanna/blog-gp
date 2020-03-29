---
title: pygtk example gtk entry icons bug (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'gtk entry icons bug'

Functions in program: 
* `def on_icon_pressed(entry, icon_pos, event):`

Modules used in program: 
* `import gtk`
* `import pygtk`

## python gtk entry icons bug

Python pygtk example: gtk entry icons bug

```python

import pygtk
pygtk.require("2.0")
import gtk

#
# Create a GTK entry with a primary and secondary icons.
# Both icons are set sensitive.
#
# If the entry is insensitive before the window is showed,
# icons will appear grayed even when the entry becomes sensitive.
#
# However, when the entry becomes sensitive, icons respond to clicks as expected. 
#

entry = gtk.Entry()

# Icons are displayed correctly when this line is commented
entry.set_sensitive(False)

entry.set_icon_from_stock(gtk.ENTRY_ICON_PRIMARY, gtk.STOCK_GO_BACK)
entry.set_icon_sensitive(gtk.ENTRY_ICON_PRIMARY, True)

entry.set_icon_from_stock(gtk.ENTRY_ICON_SECONDARY, gtk.STOCK_GO_FORWARD)
entry.set_icon_sensitive(gtk.ENTRY_ICON_SECONDARY, True)

def on_icon_pressed(entry, icon_pos, event):
    if icon_pos == gtk.ENTRY_ICON_PRIMARY:
        entry.set_text("Left")
    elif icon_pos == gtk.ENTRY_ICON_SECONDARY:
        entry.set_text("Right")

entry.connect("icon-press", on_icon_pressed)

entry.show()

#
# Create a window containing the entry widget
#

window = gtk.Window()
window.set_title("Entry icon test")
window.add(entry)
window.connect("destroy", gtk.main_quit)
window.show()

# Set entry sensitive after showing the window
entry.set_sensitive(True)

gtk.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
