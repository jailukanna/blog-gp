---
title: tkinter example gui multlist (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'gui multlist'

Functions in program: 
* `def sortby(tree, col, reverse: bool):`

Modules used in program: 
* `import tkinter.font as tkFont`
* `import tkinter.ttk as ttk`
* `import tkinter as tk`

## python gui multlist

Python tkinter example: gui multlist

```python
#!/multi_list_box.py Python3
import tkinter as tk
import tkinter.ttk as ttk
import tkinter.font as tkFont
from gui.hover_info import Pagination, pagination_style6

class MultiColumnListBox(tk.Frame):

    def __init__(self, parent, headers: list, displaycolumns: list):
        tk.Frame.__init__(self, parent)
        self.headers = headers
        self.tree = ttk.Treeview(self, column=self.headers, show="headings", displaycolumns=displaycolumns)
        self.pegination = Pagination(self, 5, 10, pagination_style=pagination_style6)

        vsb = ttk.Scrollbar(orient='vertical', command=self.tree.yview)
        hsb = ttk.Scrollbar(orient='horizontal', command=self.tree.xview)

        self.tree.configure(yscrollcommand=vsb.set, xscrollcommand=hsb.set)
        self.tree.grid(column=0, row=1, sticky='nsew', in_=self)
        self.pegination.grid(column=0, row=3)
        vsb.grid(column=1, row=1, sticky='ns', in_=self)
        hsb.grid(column=0, row=2, sticky='ew', in_=self)

        self.grid_columnconfigure(0, weight=1)
        self.grid_rowconfigure(0, weight=1)

        for header in self.headers:
            self.tree.heading(header, text=header.title(),
                              command=lambda c=header: sortby(self.tree, c, False))
            self.tree.column(header, width=tkFont.Font().measure(header.title()), anchor=tk.CENTER)

    def build_tree(self, items):

        for item in items:
            self.tree.insert('', tk.END, values=item)
            for index, value in enumerate(item):
                col_width = tkFont.Font().measure(value)
                if self.tree.column(self.headers[index], width=None) < col_width:
                    self.tree.column(self.headers[index], width=col_width)


class RightClick:
    def __init__(self, parent):

        self.tree = parent
        self.iid = None
        self.aMenu = tk.Menu(parent, tearoff=0)

        self.aMenu.add_command(label='View', command=self.view)
        self.aMenu.add_command(label='Edit', command=self.edit)
        self.aMenu.add_command(label='Delete', command=self.delete)

    def view(self):
        print('View {}'.format(self.tree.item(self.iid)['values']))

    def edit(self):
        print('Edit {}'.format(self.iid))

    def delete(self):
        self.tree.delete(self.iid)

    def popup(self, event):
        self.iid = self.tree.identify_row(event.y)
        if self.iid:
            self.tree.selection_set(self.iid)
            self.aMenu.post(event.x_root, event.y_root)


# Better to leave it as it is or do it in database!?
def sortby(tree, col, reverse: bool):
    """Sort tree contents when a column header is clicked on"""

    data = [(tree.set(child, col), child) for child in tree.get_children('')]
    data.sort(reverse=reverse)

    for ix, item in enumerate(data):
        tree.move(item[1], '', ix)

    tree.heading(col, command=lambda col=col: sortby(tree, col,
                                                     not reverse))


if __name__ == '__main__':

    from gui.test_treeview import case_data

    test_case = case_data(11)
    col_headers = ['Id', 'Wiki Id', 'Title', 'Search word', 'Added date']

    root = tk.Tk()
    listbox = MultiColumnListBox(root, col_headers, [0, 2, 3, 4])
    listbox.build_tree(test_case)
    listbox.tree.bind('<Button-3>', RightClick(listbox.tree).popup)
    listbox.pack()
    root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
