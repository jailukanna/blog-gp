---
title: pygtk example pygtk eyes (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'pygtk eyes'


Modules used in program: 
* `import glib, gtk, gobject`
* `import pygtk`

## python pygtk eyes

Python pygtk example: pygtk eyes

```python
#! /usr/bin/env python

import pygtk
pygtk.require('2.0')
import glib, gtk, gobject

from math import pi, sqrt

class Eyes(gtk.DrawingArea):
    __gsignals__ = { "expose-event": "override" }
    def __init__(self):
        Screen.__init__(self)
        glib.timeout_add(50, self.timer_tick)

    def do_expose_event(self, event):
        cr = self.window.cairo_create()
        cr.rectangle(event.area.x, event.area.y,
                event.area.width, event.area.height)
        cr.clip()
        self.draw(cr, *self.window.get_size())

    def timer_tick(self):
        alloc = self.get_allocation()
        rect = gtk.gdk.Rectangle(0, 0, alloc.width, alloc.height)
        self.window.invalidate_rect(rect, True)
        self.window.process_updates(True)
        
        return True
    
    def draw(self, cr, width, height):

        def draw_eye(offset=0):
            cr.identity_matrix()
            cr.set_source_rgb(0.0, 0.0, 0.0)
            cr.set_line_width(20)
            cr.translate(width/2. + offset, height/2.)
            cr.scale(0.85, 1.0)
            mat = cr.get_matrix()
            lx, ly = mat.transform_point(0, 0)
            lx, ly = wx+lx, wy+ly
            
            dx, dy = (mx - lx)*2, my - ly

            dist = sqrt(dx*dx + dy*dy)

            if dist > med_radius:
                dx *= med_radius/dist
                dy *= med_radius/dist

            cr.arc(0, 0, large_radius, 0, 2 * pi)
            cr.stroke()

            cr.arc(dx, dy, small_radius, 0, 2 * pi)
            cr.fill()
        
        wx, wy = self.window.get_origin()
        
        cr.set_source_rgb(0.5, 0.5, 0.5)
        cr.rectangle(0, 0, width, height)
        cr.fill()
        
        cr.set_source_rgb(1, 1, 1)
        cr.rectangle(10, 10, width - 20, height - 20)
        cr.fill()
        
        rootwin = self.get_screen().get_root_window()
        mx, my, mods = rootwin.get_pointer()

        radius = min(width * .6, height)
        
        large_radius = radius / 2.0 - 20
        small_radius = radius / 5.0 - 20

        med_radius = large_radius - small_radius - 20

        draw_eye( large_radius*.85 + 10)
        draw_eye(-large_radius*.85 - 10)
            
if __name__ == "__main__":
    window = gtk.Window()
    window.connect("delete-event", gtk.main_quit)
    widget = Eyes()
    widget.show()
    window.add(widget)
    window.present()
    gtk.main()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
