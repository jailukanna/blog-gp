---
title: tkinter example widgets multi list box (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'widgets multi list box'

Functions in program: 
* `def sortby(tree, col, reverse: bool):`

Modules used in program: 
* `import tkinter.font as tkFont`
* `import tkinter.ttk as ttk`
* `import tkinter as tk`

## python widgets multi list box

Python tkinter example: widgets multi list box

```python
#!/multi_list_box.py Python3
import tkinter as tk
import tkinter.ttk as ttk
import tkinter.font as tkFont


class MultiColumnListBox(tk.Frame):

    def __init__(self, parent, headers: list, displaycolumns: list):
        tk.Frame.__init__(self, parent)
        self.headers = headers
        #self._create_configuration_of_style()
        self.tree = ttk.Treeview(self, column=self.headers, show="headings", displaycolumns=displaycolumns)

        vsb = ttk.Scrollbar(orient='vertical', command=self.tree.yview)
        hsb = ttk.Scrollbar(orient='horizontal', command=self.tree.xview)

        self.tree.configure(yscrollcommand=vsb.set, xscrollcommand=hsb.set)
        self.tree.grid(column=0, row=1, sticky='nsew', in_=self)

        vsb.grid(column=1, row=1, sticky='ns', in_=self)
        hsb.grid(column=0, row=2, sticky='ew', in_=self)

        self.grid_columnconfigure(0, weight=1)
        self.grid_rowconfigure(0, weight=1)

        for header in self.headers:
            self.tree.heading(header, text=header.title(),
                              command=lambda c=header: sortby(self.tree, c, False))
            self.tree.column(header, width=tkFont.Font().measure(header.title()), anchor=tk.CENTER)

    def _create_configuration_of_style(self):
        style = ttk.Style()
        style.configure("mystyle.Treeview", highlightthickness=0, bd=0, font=('Calibri', 11)) # Modify the font of the body
        style.configure("mystyle.Treeview.Heading", font=('Calibri', 13,'bold')) # Modify the font of the headings
        style.layout("mystyle.Treeview", [('mystyle.Treeview.treearea', {'sticky': 'nswe'})]) # Remove the borders

    def build_tree(self, items):

        for row, item in enumerate(items):
            self.tree.insert('', tk.END, values=item, tags=('odd' if row % 2 == 0 else 'even', ))
            for index, value in enumerate(item):
                col_width = tkFont.Font().measure(value)
                if self.tree.column(self.headers[index], width=None) < col_width:
                    self.tree.column(self.headers[index], width=col_width)

        self.tree.tag_configure('odd', background='#E8E8E8', font=("Verdana", 8))
        self.tree.tag_configure('even', background='#DFDFDF', font=("Verdana", 8))


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
