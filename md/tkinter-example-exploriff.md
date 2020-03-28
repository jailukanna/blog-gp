---
title: tkinter example exploriff (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'exploriff'

Functions in program: 
* `def main():`

Modules used in program: 
* `import tkinter.ttk as ttk`
* `import tkinter as tk`
* `import sys, struct`

## python exploriff

Python tkinter example: exploriff

```python
#!/usr/bin/env python3

# GUI to explore the tree structure of RIFF files
# use in conjunction with an hex editor to explore raw data at the leaves

# RIFF format specification:
# https://msdn.microsoft.com/en-us/library/dd798636(v=vs.85).aspx

import sys, struct
import tkinter as tk
import tkinter.ttk as ttk


Internal_ID = [b'RIFF',b'LIST']

class RIFF:
    def read_uint(self):
        return struct.unpack('<I',self.File.read(4))[0]
    
    def read_string(self,s=4):
        return struct.unpack('%ds'%s,self.File.read(s))[0]
    
    def __init__(self,f,o=None):
        self.File = f
        self.Offset = 0 if o==None else o
        self.File.seek(self.Offset)
        self.ID = self.read_string()
        self.Size = self.read_uint()
        self.Type = b''
        self.Chunks = []
        if self.ID in Internal_ID and self.Size>=4:
            self.Type = self.read_string()
            o = self.Offset+12
            while o<self.Offset+self.Size:
                self.Chunks.append(RIFF(self.File,o))
                o += self.Chunks[-1].Size+8
    
    def str_id(self):
        return ascii(self.ID)[2:-1]
    
    def str_offset(self):
        return '%d (0x%X)' % (self.Offset,self.Offset)
    
    def str_type(self):
        return ascii(self.Type)[2:-1]


class RIFF_GUI(tk.Frame):
    def __init__(self, master=None):
        tk.Frame.__init__(self, master)
        top = self.winfo_toplevel()
        top.rowconfigure(0,weight=1)
        top.columnconfigure(0,weight=1)
        self.grid(sticky=tk.N+tk.S+tk.E+tk.W)
        self.rowconfigure(0, weight=1)
        self.columnconfigure(0, weight=1)
        Cols = ['ID','Offset','Size','Type']
        self.tview = ttk.Treeview(self,columns=tuple('#%d'%c for c in range(1,len(Cols))))
        for c in range(len(Cols)):
            self.tview.heading('#%d'%c,text=Cols[c])
        self.sby = ttk.Scrollbar(self,orient='vertical',command=self.tview.yview)
        self.tview['yscrollcommand'] = self.sby.set
        self.tview.grid(row=0,column=0,sticky=tk.N+tk.S+tk.E+tk.W)
        self.sby.grid(row=0,column=1,sticky=tk.N+tk.S)
    
    def gen_tree(self, R, u=''):
        node = self.tview.insert(u,'end',text=R.str_id(),values=(R.str_offset(),str(R.Size),R.str_type()),open=(u==''))
        for C in R.Chunks:
            self.gen_tree(C,node)


def main():
    if len(sys.argv)<2:
        sys.stderr.write('usage: python3 %s /path/to/riff_file\n' % sys.argv[0])
        sys.exit(1)
    try:
        f = open(sys.argv[1],'rb')
    except:
        sys.stderr.write('Error opening file\n')
        sys.exit(1)
    try:
        R = RIFF(f)
    except:
        sys.stderr.write('Error parsing file\n')
        sys.exit(1)
    f.close()
    root = tk.Tk()
    root.style = ttk.Style()
    root.style.theme_use('clam')
    gui = RIFF_GUI(master=root)
    gui.master.title('RIFF Explorer')
    gui.gen_tree(R)
    gui.mainloop()

main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
