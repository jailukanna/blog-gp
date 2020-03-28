---
title: tkinter example show-ip (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'show-ip'


Modules used in program: 
* `import netifaces`
* `import Tkinter`

## python show-ip

Python tkinter example: show-ip

```python
#!/usr/bin/python2
# Network Code from http://stackoverflow.com/questions/11735821/python-get-localhost-ip

import Tkinter
import netifaces

class show_ip_tk(Tkinter.Tk):
    def __init__(self,parent):
        Tkinter.Tk.__init__(self,parent)
        self.parent = parent
        self.getip()
        self.initialize()

#Get IP Adress
    def getip(self):
        interfaces = netifaces.interfaces()
        for i in interfaces:
            if i == 'lo':
            		self.ip = "   Not connected   "
                continue
            iface = netifaces.ifaddresses(i).get(netifaces.AF_INET)
            if iface != None:
                for j in iface:
                    self.ip = j['addr']

    def initialize(self):
        self.grid()

        button = Tkinter.Button(self,text=u"Reload",
                                command=self.OnButtonClick)
        button.grid(column=1,row=0)

        self.labelVariable = Tkinter.StringVar()
        label = Tkinter.Label(self,textvariable=self.labelVariable,font=("Noto Sans UI", 50))
        label.grid(column=0,row=0,sticky='EW')
        self.labelVariable.set(self.ip)

        self.grid_columnconfigure(0,weight=1)
        self.resizable(True,False)
        self.update()
        self.geometry(self.geometry())      

    def OnButtonClick(self):
	self.getip()
        self.labelVariable.set(self.ip)

if __name__ == "__main__":
    app = show_ip_tk(None)
    app.title('Show IP Adress')
    app.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
