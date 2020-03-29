---
title: pygtk example celldatafunc headers (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'celldatafunc headers'

Functions in program: 
* `def main():`

Modules used in program: 
* `import gtk`
* `import pygtk`

## python celldatafunc headers

Python pygtk example: celldatafunc headers

```python
import pygtk
pygtk.require('2.0')
import gtk

data = [
    [('val_a1', 'val_b1', 'val_c1', 'val_d1', 'val_e1'), ('', 'val_x1', 'val_y1', 'val_z1', ''), ('', 'val_x2', 'val_y2', 'val_z2', '')],
    [('val_a2', 'val_b2', 'val_c2', 'val_d2', 'val_e2'), ('', 'val_x3', 'val_y3', 'val_z3', '')],
    [('val_a3', 'val_b3', 'val_c3', 'val_d3', 'val_e3')],
    [('val_a4', 'val_b4', 'val_c4', 'val_d4', 'val_e4'), ('', 'val_x4', 'val_y4', 'val_z4', ''), ('', 'val_x5', 'val_y5', 'val_z5', '')],
]

class BasicTreeViewExample:
    
    parent_headers = ("P.Col 1", "P.Col 2", "P.Col 3", "P.Col 4", "P.Col 5")
    child_headers = ("C.Col 1", "C.Col 2", "C.Col 3")
    
    def delete_event(self, widget, event, data=None):
        gtk.main_quit()
        return False

    def __init__(self):
        self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
        self.window.set_title("Basic TreeView Example")
        self.window.set_size_request(200, 200)
        self.window.connect("delete_event", self.delete_event)
        self.treestore = gtk.TreeStore(str, str, str, str, str)
        for detail in data:
            for index, elem in enumerate(detail):
                if index == 0:
                    piter = self.treestore.append(None, elem)
                else:
                    self.treestore.append(piter, elem)

        self.treeview = gtk.TreeView(self.treestore)
        for i in range(5):
            tvcolumn = gtk.TreeViewColumn('Column %s' % (i))
            self.treeview.append_column(tvcolumn)
            cell = gtk.CellRendererText()
            tvcolumn.pack_start(cell, True)
            # Delegate data fetching to callback
            tvcolumn.set_cell_data_func(cell, self.cell_add_header, i)
        self.window.add(self.treeview)
        self.window.show_all()
    
    def cell_add_header(self, treeviewcolumn, cell, model, iter_, column):
        text = model.get_value(iter_, column)
        if model.iter_parent(iter_) is None:
            # This is a parent
            title = self.parent_headers[column]
            markup = "<b>%s</b>\n%s"%(title, text)
        else:
            # We have a child
            if column == 0 or column == 4:
                # Cell is not used by child, leave it empty
                markup = ""
            else:
                title = self.child_headers[column-1]
                markup = "<b>%s</b>\n%s"%(title, text)
        cell.set_property('markup', markup)

def main():
    gtk.main()

if __name__ == "__main__":
    tvexample = BasicTreeViewExample()
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
