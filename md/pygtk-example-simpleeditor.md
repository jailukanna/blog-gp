---
title: pygtk example simpleeditor (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'simpleeditor'

Functions in program: 
* `def main():`

Modules used in program: 
* `import gtk`
* `import pygtk`

## python simpleeditor

Python pygtk example: simpleeditor

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

"""
Až moc jednoduchý textový editor.
Vlevo je seznam úryvků, dvojklikem na nějaký se úryvek vloží do textu.
"""

import pygtk
pygtk.require("2.0")
import gtk


class SimpleEditor:

    def __init__(self):
        self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        self.window.connect("destroy", self.destroy)
        self.window.set_title("Simple editor example")
        self.window.set_default_size(400, 300)

        self.hpan = gtk.HPaned()
        self.window.add(self.hpan)

        self.snippets = [
            "Lorem ipsum dolor sit amet.",
            "int main(int argc, char *argv[]) {\n}\n",
        ]

        self.treestore = gtk.ListStore(str, str)
        for snippet in self.snippets:
            name = snippet[:20]
            self.treestore.append([snippet, name])

        self.treeview = gtk.TreeView(self.treestore)
        self.tvcol = gtk.TreeViewColumn("Snippets")
        self.treeview.append_column(self.tvcol)
        cell = gtk.CellRendererText()
        self.tvcol.pack_start(cell, True)
        self.tvcol.add_attribute(cell, "text", 1)
        self.treeview.connect("button-press-event", 
            self.on_treeview_doubleclick)

        self.hpan.add1(self.treeview)


        self.tvsw = gtk.ScrolledWindow()
        self.tvsw.set_policy(gtk.POLICY_AUTOMATIC, gtk.POLICY_AUTOMATIC)
        self.hpan.add2(self.tvsw)

        self.textview = gtk.TextView()
        self.tvsw.add(self.textview)

        self.window.show_all()


    def destroy(self, widget, data=None):
        gtk.main_quit()


    def main(self):
        gtk.main()


    def on_treeview_doubleclick(self, widget, event, data=None):
        if event.type != gtk.gdk._2BUTTON_PRESS:
            return
        model, treeIter = self.treeview.get_selection().get_selected()
        if treeIter is None:
            return  # no row is selected
        snippet = model.get_value(treeIter, 0)
        self.textview.get_buffer().insert_at_cursor(snippet)


def main():
    se = SimpleEditor()
    se.main()


if __name__ == "__main__":
    main()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
