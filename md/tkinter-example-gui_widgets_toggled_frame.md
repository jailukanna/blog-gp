---
title: tkinter example gui widgets toggled frame (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'gui widgets toggled frame'


Modules used in program: 
* `import tkinter.ttk as ttk`
* `import tkinter as tk`

## python gui widgets toggled frame

Python tkinter example: gui widgets toggled frame

```python
#!/toggled_frame.py Python3
import tkinter as tk
import tkinter.ttk as ttk


class ToggledFrame(tk.Frame):

    def __init__(self, parent, text='', sep=('-', '+'), *args, **options):
        tk.Frame.__init__(self, parent, *args, **options)

        self.show = tk.IntVar()
        self.show.set(0)
        self.sep = sep
        self.title_frame = ttk.Frame(self)
        self.title_frame.pack(expand=True, side=tk.TOP)

        ttk.Label(self.title_frame, text=text).pack(side=tk.LEFT, fill=tk.X, expand=True)

        self.toggle_button = ttk.Checkbutton(self.title_frame, width=2, text='+', command=self.toggle,
                                             variable=self.show, style='Toolbutton')

        self.toggle_button.pack(side="left")

        self.sub_frame = tk.Frame(self, relief=tk.SUNKEN, borderwidth=1)

    def toggle(self) -> None:
        """Show and hide toggled frame."""
        if bool(self.show.get()):
            self.sub_frame.pack(fill=tk.X, expand=True)
            self.toggle_button.configure(text=self.sep[0])
        else:
            self.sub_frame.forget()
            self.toggle_button.configure(text=self.sep[1])


if __name__ == '__main__':
    root = tk.Tk()
    root.geometry('300x200')

    tf = ToggledFrame(root, text='Toggled Filter')
    tf.pack(fill=tk.X, pady=2, padx=2, anchor=tk.N)

    filter_names = ['Table Id', 'Wiki Id', 'Title', 'Search word', 'Added date']

    for index, name in enumerate(filter_names):
        tk.Entry(tf.sub_frame).grid(row=index, column=1, pady=1, padx=1)
        tk.Label(tf.sub_frame, text=name).grid(row=index, column=0, pady=1, padx=1, sticky=tk.W)

    btn = tk.Button(tf.sub_frame, text='Filter').grid(row=len(filter_names), column=2)

    root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
