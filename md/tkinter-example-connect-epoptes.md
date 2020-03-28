---
title: tkinter example connect-epoptes (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'connect-epoptes'


Modules used in program: 
* `import netifaces`
* `import subprocess`
* `import Tkinter`

## python connect-epoptes

Python tkinter example: connect-epoptes

```python
#!/usr/bin/python2
#Connect to an Epoptes server

import Tkinter
import subprocess
import netifaces

class connect_tk(Tkinter.Tk):
    def __init__(self,parent):
        Tkinter.Tk.__init__(self,parent)
        self.parent = parent
	self.getlocalip()
        self.initialize()

    def getlocalip(self):
        interfaces = netifaces.interfaces()
        for i in interfaces:
            if i == 'lo':
		self.localip = "127.0.0.1"
                continue
            iface = netifaces.ifaddresses(i).get(netifaces.AF_INET)
            if iface != None:
                for j in iface:
                    self.localip = j['addr']

#Connect to an Epoptes server
    def connect(self, epoptesip):
    	print(epoptesip)
	    subprocess.check_call(["/usr/sbin/epoptes-client", str(epoptesip)])

    def initialize(self):
        self.grid()

        self.entryVariable = Tkinter.StringVar()
        self.entry = Tkinter.Entry(self,textvariable=self.entryVariable,font=("Noto Sans UI", 30))
        self.entry.grid(column=0,row=0,sticky='EW')
        self.entry.bind("<Return>", self.OnPressEnter)
        self.entryVariable.set(self.localip)

        button = Tkinter.Button(self,text=u"Connect!",
                                command=self.OnButtonClick,font=("Noto Sans UI", 30))
        button.grid(column=1,row=0)

        self.labelVariable = Tkinter.StringVar()
        label = Tkinter.Label(self,textvariable=self.labelVariable,font=("Noto Sans UI", 30))
        label.grid(column=0,row=1,columnspan=2,sticky='EW')
        self.labelVariable.set(u"Not connected")

        self.grid_columnconfigure(0,weight=1)
        self.resizable(True,False)
        self.update()
        self.geometry(self.geometry())       
        self.entry.focus_set()
        self.entry.selection_range(0, Tkinter.END)

    def OnButtonClick(self):
      	self.connect(self.entryVariable.get())
        self.labelVariable.set( "Connected to " + self.entryVariable.get() )
        self.entry.focus_set()
        self.entry.selection_range(0, Tkinter.END)

    def OnPressEnter(self,event):
      	self.connect(self.entryVariable.get())
        self.labelVariable.set( "Connected to " + self.entryVariable.get() )
        self.entry.focus_set()
        self.entry.selection_range(0, Tkinter.END)

if __name__ == "__main__":
    app = connect_tk(None)
    app.title('Connect to Epoptes')
    app.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
