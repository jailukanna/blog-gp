---
title: tkinter example checkabletreeview (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'checkabletreeview'


## python checkabletreeview

Python tkinter example: checkabletreeview

```python
try:
    import tkinter.ttk as ttk
except ImportError:
    import ttk


class CheckableTreeview(ttk.Treeview):
    check_mark = '✓'  # '*'
    uncheck_mark = ''

    def __init__(self, *args, **kwargs):
        super(CheckableTreeview, self).__init__(*args, **kwargs)

        self.check_col = None
        self.bind('<Button-1>', self._click)

    def set_check_col(self, col_id):
        self.check_col = col_id

    def get_checked_items(self):
        pass

    def _click(self, event):
        if self.identify_region(event.x, event.y) == 'cell':
            # selected_item = self.selection()

            # print(self.item(selected_item))
            row = self.identify_row(event.y)
            column = self.identify_column(event.x)
            if self.set(row) and self.column(column)['id'] == self.check_col:
                if self.set(row, self.check_col) != self.check_mark:
                    self.set(row, self.check_col, self.check_mark)
                else:
                    self.set(row, self.check_col, self.uncheck_mark)


if __name__ == '__main__':
    try:
        import tkinter as tk
    except ImportError:
        import Tkinter as tk

    root = tk.Tk()
    tree = CheckableTreeview(root)

    tree["columns"] = ("one", "two")
    tree.column("one", width=100)
    tree.column("two", width=100)
    tree.heading("one", text="coulmn A")
    tree.heading("two", text="column B")
    tree.set_check_col('two')

    tree.insert("", 0, text="Line 1", values=("1A", "1b"))

    id2 = tree.insert("", 1, "dir2", text="Dir 2")
    sub = tree.insert(id2, "end", "dir 2", text="sub dir 2", values=("2A", "2B"))

    ##alternatively:
    tree.insert("", 3, "dir3", text="Dir 3")
    tree.insert("dir3", 3, text=" sub dir 3", values=("3A", " 3B"))

    tree.pack()
    root.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
