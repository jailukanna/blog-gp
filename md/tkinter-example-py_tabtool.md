---
title: tkinter example py tabtool (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'py tabtool'


Modules used in program: 
* `import pandas as pd`
* `import tkinter.ttk as ttk`
* `import tkinter as tk`

## python py tabtool

Python tkinter example: py tabtool

```python
#!/bin/python3
#coding: UTF-8

import tkinter as tk
import tkinter.ttk as ttk
from tkinter import messagebox
from tkinter import filedialog
import pandas as pd

class Application(tk.Frame):
    def __init__(self, master=None):
        super().__init__(master)
        master.title("tkinter.ttk::Notebook::Tab")
        master.geometry("800x400")
        master.bind("<Escape>", self.exit_app)
        master.bind("<Alt-c>", self.exit_app)
        master.bind("<Alt-r>", self.read_params)
        master.bind("<Alt-o>", self.change_command)
        self.create_widgets()

    def create_widgets(self): 
        #main window
        self.pack(expand=True, fill="both", anchor="n", padx=5, pady=5)

        #setup::header area
        self.frm_header = tk.Frame(self, bd=5)
        self.dl_command = ttk.Combobox(self.frm_header, state='readonly')
        self.dl_command["values"] = ("Item1", "Item2", "Item3") 
        self.dl_command.current(0)
        self.dl_command.bind("<<ComboboxSelected>>", self.change_command)
        self.dl_command.pack(side="right") 
        self.lb_command = tk.Label(self.frm_header, text="Command", underline=1)
        self.lb_command.pack(side="right", fill="x") 
        self.frm_header.pack(expand=False, fill="both", side="top")

        #setup::create tab
        self.nb = ttk.Notebook(self)
        self.tb_general = tk.Frame(self.nb)
        self.tb_import  = tk.Frame(self.nb)
        self.nb.add(self.tb_general, text="General")
        self.nb.add(self.tb_import, text="Import")
        self.nb.pack(expand=True, fill="both", side="top")

        #[tab1] General::create widgets
        self.tree1 = ttk.Treeview(self.tb_general)
        self.tree1["columns"] = (1,2,3)
        self.tree1["show"] = "headings"
        self.tree1.heading(1, text="A")
        self.tree1.heading(2, text="B")
        self.tree1.heading(3, text="C")
        self.tree1.insert("", "end", values=("A1", "B1", "C1"))
        self.tree1.insert("", "end", values=("A2", "B2", "C2"))
        self.tree1.insert("", "end", values=("A3", "B3", "C3"))
        self.tree1.pack(expand=True, fill="both")
        #[tab2] Import::create widgets
        self.tree2 = ttk.Treeview(self.tb_import)
        self.tree2["columns"] = (1,2,3)
        self.tree2["show"] = "headings"
        self.tree2.heading(1, text="D")
        self.tree2.heading(2, text="E")
        self.tree2.heading(3, text="F")
        self.tree2.insert("", "end", values=("D1", "E1", "F1"))
        self.tree2.insert("", "end", values=("D2", "E2", "F2"))
        self.tree2.insert("", "end", values=("D3", "E3", "F3"))
        self.tree2.pack(expand=True, fill="both")

        #setup::footer area
        self.frm_footer = tk.Frame(root, bd=5, relief = tk.GROOVE)
        self.bt_save  = tk.Button(self.frm_footer, text="Save", underline=0, command=self.save_params)
        self.bt_read  = tk.Button(self.frm_footer, text="Read", underline=0, command=self.read_params)
        self.bt_close = tk.Button(self.frm_footer, text="Close", underline=0, command=self.exit_app)
        self.lb_info  = tk.Label(self.frm_footer, relief = tk.RIDGE)
        self.bt_save   .pack(side="right") 
        self.bt_read   .pack(side="right") 
        self.bt_close  .pack(side="right") 
        self.lb_info   .pack(side="right", fill="x") 
        self.frm_footer.pack(expand=False, fill="x", side="top")

    def exit_app(self, event=None):
        print("[exit_app]")
        if(messagebox.askokcancel("py_tab", "exit ?")):
            self.quit()
    
    def save_params(self, event=None):
        print("[save_params]")
    
    def read_params(self, event=None):
        print("[read_params]")
        filetype_list = [("csv file", ".csv"), ("all file", "*")]
        filepath = filedialog.askopenfilename(initialdir="./", filetypes=filetype_list, title="select file")
        print(filepath)
       
        #read csv file and add list 
        df = pd.read_csv(filepath, sep=",")
        print(df.query("f==1"))
      
        #read csv file and add list 
        df = pd.read_csv(filepath, sep=",", index_col=0, usecols=lambda x: x not in ["OPTIONS_DEFAULT_VALUE"])
        print(df)

    def change_command(self, event=None):
        print("[change_command]")

root = tk.Tk()
app = Application(master=root)
app.mainloop() 


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
