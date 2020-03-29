---
title: pygtk example book (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'book'


Modules used in program: 
* `import gobject`
* `import gtk`
* `import pygtk`

## python book

Python pygtk example: book

```python
#!/usr/bin/env python

import pygtk
pygtk.require('2.0')
import gtk
import gobject

BOOKING_MINUTES = 60


class Book:

    def __init__(self):
        self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        self.window.set_keep_above(True)
        self.window.set_title("Boka")
        self.window.set_type_hint(gtk.gdk.WINDOW_TYPE_HINT_MENU)
        gobject.timeout_add(3000, self.always_on_top_hack)
        self.minutes = 0
        self.window.connect("delete_event", self.delete_event)
        self.window.connect("destroy", self.destroy)
        self.window.set_border_width(15)
        self.button = gtk.Button("BOOK BUTTON")
        self.button.connect("clicked", self.toggle_booked, None)
        self.label = gtk.Label()
        self.label.set_use_markup(True)
        self.ledig()
        vbox = gtk.VBox()
        vbox.add(self.button)
        vbox.add(self.label)
        self.window.add(vbox)
        self.window.show_all()
        self.window.move(50, 50)
        width, height = self.window.get_size()
        self.window.move(0, gtk.gdk.screen_height() - height)

    def toggle_booked(self, widget, data=None):
        if self.minutes == 0:
            self.minutes = BOOKING_MINUTES
            self.button.set_label('Avbryt')
            self.update_minutes()
            self.timeout_id = gobject.timeout_add(BOOKING_MINUTES,
                                                  self.update_minutes)
        else:
            self.cancel_booking()

    def cancel_booking(self):
        self.minutes = 0
        gobject.source_remove(self.timeout_id)
        self.ledig()

    def update_minutes(self):
        markup = "<span size='large' background='red'>\
Bokad %d min</span>" % self.minutes
        self.label.set_markup(markup)
        self.minutes = self.minutes - 1
        if self.minutes == 0:
            self.cancel_booking()
            return False
        else:
            return True

    def ledig(self):
        self.button.set_label('Boka %d minuter' % BOOKING_MINUTES)
        self.label.set_markup("<span size='large' background='#00ff00'>\
Ledig</span>")

    def delete_event(self, widget, event, data=None):
        print("delete event occurred")
        return False

    def destroy(self, widget, data=None):
        print("destroy signal occurred")
        gtk.main_quit()

    def always_on_top_hack(self):
        self.window.set_keep_above(False)
        self.window.set_keep_above(True)
        return True

    def main(self):
        gtk.main()


if __name__ == "__main__":
    w = Book()
    w.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
