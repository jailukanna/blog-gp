---
title: pygtk example cairo-cat (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'cairo-cat'

Functions in program: 
* `def run(size=(200, 200)):`
* `def deg2rad(x):`

Modules used in program: 
* `import math`
* `import gtk, gobject, cairo`
* `import pygtk`

## python cairo-cat

Python pygtk example: cairo-cat

```python
#!/usr/bin/python2

import pygtk
pygtk.require('2.0')
import gtk, gobject, cairo

import math

def deg2rad(x):
    return x * math.pi / 180

class Screen(gtk.DrawingArea):
    # Draw in response to an expose-event
    __gsignals__ = { "expose-event": "override" }

    def do_expose_event(self, event):
        # Create the cairo context
        cr = self.window.cairo_create()

        # Restrict Cairo to the exposed area; avoid extra work
        cr.rectangle(event.area.x, event.area.y,
                event.area.width, event.area.height)
        cr.clip()

        self.draw(cr, *self.window.get_size())

    def draw(self, cr, width, height):
        cr.save()
        cr.translate(width / 2, height / 2)

        radius = 40
        ear_angle1 = 0
        ear_angle2 = 75
        ear_offset = radius
        ear_height = 60

        cr.arc(0, 0, radius, deg2rad(-ear_angle1), deg2rad(180 + ear_angle1))
        cr.line_to(-ear_offset, -ear_height)
        cr.arc(0, 0, radius, deg2rad(180 + ear_angle2), deg2rad(-ear_angle2))
        cr.line_to(ear_offset, -ear_height)
        cr.close_path()

        cr.set_source_rgb(1.0, 1, 0)
        cr.fill_preserve()
        cr.set_source_rgb(1.0, 0.75, 0)
        cr.set_line_width(5)
        cr.stroke()

        cr.restore()


def run(size=(200, 200)):
    gtk.threads_init()
    gtk.threads_enter()
    
    window = gtk.Window()

    widget = Screen()
    widget.show()

    window.connect("delete-event", gtk.main_quit)
    window.add(widget)
    #window.set_decorated(False)
    #window.set_skip_taskbar_hint(True)
    #window.set_skip_pager_hint(True)
    #window.set_keep_above(True)
    #window.stick()
    window.set_default_size(*size)

    window.present()
    try:
        gtk.main()
    finally:
        gtk.threads_leave()

if __name__ == "__main__":
    run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
