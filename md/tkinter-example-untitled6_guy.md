---
title: tkinter example untitled6 guy (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'untitled6 guy'


Modules used in program: 
* `import op`
* `import tkinter as tk`

## python untitled6 guy

Python tkinter example: untitled6 guy

```python
import tkinter as tk
import op


class trabMDApp(tk.Tk):
    mdcab=0
    def __init__(self):
        tk.Tk.__init__(self)
        self.entrya = tk.Entry(self)
        self.entryb = tk.Entry(self)
        self.buttonmdc = tk.Button(self, text="MDC", command=self.mdc_button)
        self.buttonmdcx = tk.Button(self, text="MDC Extendido", command=self.mdcx_button)
        self.buttondiv = tk.Button(self,text="Divisao e resto", command=self.divr_button)
        self.buttonmmc = tk.Button(self,text="MMC", command=self.mmc_button)
        self.Tela = tk.Text(self)
        self.Tela.pack()
        self.buttondiv.pack()
        self.buttonmdc.pack()
        self.buttonmmc.pack()
        self.buttonmdcx.pack()
        self.entrya.pack()
        self.entryb.pack()

    def divr_button(self):
        a = int(str(self.entrya.get()))
        b = int(str(self.entryb.get()))
        div, resto= op.div(a,b)
        self.Tela.delete('1.0', "end")
        self.Tela.insert("end",("Divisao = %d \nResto = %d" % (div, resto)))


    def mdc_button(self):
        a = int(str(self.entrya.get()))
        b = int(str(self.entryb.get()))
        mdcab = op.mdc_r(a,b)
        #print(mdcab)
        self.Tela.delete('1.0', "end")
        self.Tela.insert("end",mdcab)

    def mmc_button(self):
        a = int(str(self.entrya.get()))
        b = int(str(self.entryb.get()))
        mmcresul = op.mmc(a,b)
        #print(mdcab)
        self.Tela.delete('1.0', "end")
        self.Tela.insert("end",mmcresul)

    def mdcx_button(self):
        a = int(str(self.entrya.get()))
        b = int(str(self.entryb.get()))
        x,y,d = op.xmdc(a,b)
        #print("d=ax+by ---> %d = (%d)*(%d) + (%d)*(%d)" % (d,a,x,b,y))
        self.Tela.delete('1.0', "end")
        self.Tela.insert("end",("d=ax+by ---> %d = (%d)*(%d) + (%d)*(%d)" % (d,a,x,b,y)))



root = trabMDApp()
root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
