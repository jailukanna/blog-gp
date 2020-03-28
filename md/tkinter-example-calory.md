---
title: tkinter example calory (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'calory'


Modules used in program: 
* `import tkinter.ttk as ttk`
* `import tkinter as tk`

## python calory

Python tkinter example: calory

```python
#coding UTF-8

import tkinter as tk
import tkinter.ttk as ttk



class CaloryApp(ttk.Frame):
    def __init__(self,master=None):
        super().__init__(master)
        self.kinds()
        self.intake()
        self.result()
        self.widgets()
        self.grid(column=0,row=0,sticky=(tk.N,tk.E,tk.S,tk.W))
        #selfで自分自身を参照、class内を探る
        #これがないと配置がされない

    
    def kinds(self):
        Label=ttk.Label(self,text="運動の種類")
        Label.grid(column=0,row=0,sticky=(tk.N,tk.E,tk.S,tk.W))
        ttk.Label(self,text='ジョギング').grid(column=2,row=0,sticky=(tk.N,tk.E,tk.S,tk.W))
        ttk.Label(self,text='ウォーキング').grid(column=3,row=0,sticky=(tk.N,tk.E,tk.S,tk.W))
        ttk.Label(self,text='軽い筋トレ').grid(column=4,row=0,sticky=(tk.N,tk.E,tk.S,tk.W))
        self.display_var=tk.StringVar()
        self.display_var.set('40')
        ttk.Label(self,textvariable=self.display_var).grid(column=0,row=2)        
        self.display_var4=tk.StringVar()
        self.display_var4.set('0')
        ttk.Label(self,textvariable=self.display_var4).grid(column=3,row=2)
        self.display_var5=tk.StringVar()
        self.display_var5.set('20')
        ttk.Label(self,textvariable=self.display_var5).grid(column=6,row=2)
        ttk.Label(self,text='分').grid(column=9,row=2)
        self.display_var6=tk.StringVar()
        self.display_var6.set('0')
        ttk.Label(self,textvariable=self.display_var6).grid(column=1,row=3,sticky=(tk.N,tk.S,tk.E,tk.W))
        button1=ttk.Button(self,text='▲')
        button1.grid(column=1,row=2,sticky=(tk.N,tk.E,tk.S,tk.W))
        
        ttk.Button(self,text='▼').grid(column=2,row=2,sticky=(tk.N,tk.E,tk.S,tk.W))
        ttk.Button(self,text='▲').grid(column=4,row=2,sticky=(tk.N,tk.E,tk.S,tk.W))
        ttk.Button(self,text='▼').grid(column=5,row=2,sticky=(tk.N,tk.E,tk.S,tk.W))
        ttk.Button(self,text='▲').grid(column=7,row=2,sticky=(tk.N,tk.E,tk.S,tk.W))
        ttk.Button(self,text='▼').grid(column=8,row=2,sticky=(tk.N,tk.E,tk.S,tk.W))

        
    def intake(self):
        label1=ttk.Label(self,text="摂取カロリー")
        label1.grid(column=0,row=4,sticky=(tk.N,tk.E,tk.S,tk.W))
        self.display_var3=tk.StringVar()
        self.display_var3.set('1000')
        ttk.Label(self,textvariable=self.display_var3).grid(column=1,row=5,sticky=(tk.N,tk.E,tk.S,tk.W))
        ttk.Label(self,text='kcal').grid(column=2,row=5,sticky=(tk.N,tk.E,tk.S,tk.W))
        ttk.Button(self,text='▲').grid(column=3,row=5,sticky=(tk.N,tk.E,tk.S,tk.W))
        ttk.Button(self,text='▼').grid(column=4,row=5,sticky=(tk.N,tk.E,tk.S,tk.W))
        label2=ttk.Label(self,text='kcal')
        label2.grid(column=2,row=3,sticky=(tk.N,tk.S,tk.E,tk.W))

        

    def result(self):
        label2=ttk.Label(self,text="総計")
        label2.grid(column=0,row=6,sticky=(tk.N,tk.E,tk.S,tk.W))
        self.display_var2=tk.StringVar()
        self.display_var2.set('0')
        label3=ttk.Label(self,textvariable=self.display_var2)
        label3.grid(column=1,row=7,columnspan=4,sticky=(tk.N,tk.S,tk.E,tk.W))
        label4=ttk.Label(self,text='kcal')
        label4.grid(column=2,row=7,sticky=(tk.N,tk.S,tk.E,tk.W))
        ttk.Button(self,text='計算').grid(column=4,row=7,sticky=(tk.N,tk.S,tk.E,tk.W))


        

    def widgets(self):
         #各列の引き延ばし設定
        self.columnconfigure(6,weight=1)

        #各行の引き延ばし設定
        self.rowconfigure(0,weight=1)
        self.rowconfigure(1,weight=1)
        self.rowconfigure(3,weight=1)
        self.rowconfigure(4,weight=1)
        self.rowconfigure(6,weight=1)

        #トップレベルのウィジェットも引き延ばしに対応させる
        self.master.columnconfigure(0,weight=1)
        self.master.rowconfigure(0,weight=1)

    

        
       


root=tk.Tk()
root.title('カロリー計算')
root.geometry("700x500")
    #横×縦
    #Entryは基本改行できない
CaloryApp(root)
root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
