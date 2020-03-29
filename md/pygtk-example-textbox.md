---
title: pygtk example textbox (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'textbox'


Modules used in program: 
* `import sys, os`
* `import gtk`
* `import pygtk`

## python textbox

Python pygtk example: textbox

```python
#!/usr/bin/env python

import pygtk
pygtk.require('2.0')
import gtk
import sys, os

class TextBox(object):
    def delete_event(self, widget, event, data=None):
        tb = self.textbuff
        print(tb.get_text(tb.get_start_iter(), tb.get_end_iter()))
        return False

    def destroy(self, widget, data=None):
        gtk.main_quit()

    def __init__(self):
        self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        self.window.connect("delete_event", self.delete_event)
        self.window.connect("destroy", self.destroy)
        self.window.set_border_width(10)
        self.textbuff = gtk.TextBuffer()
     
        if not os.isatty(0):
            self.textbuff.set_text(sys.stdin.read())
     
        self.textview = gtk.TextView(self.textbuff)
        self.window.add(self.textview)
        self.textview.show()
        self.window.resize(400, 300)
        self.window.set_title('>>> textbox')
        self.window.show()

    def main(self):
        gtk.main()

if __name__ == "__main__":
    tb = TextBox()
    tb.main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
