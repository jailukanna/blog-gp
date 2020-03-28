---
title: tkinter example frame3 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'frame3'


Modules used in program: 
* `import tkinter`

## python frame3

Python tkinter example: frame3

```python
#frame3.py some frame work soem altering of frame2.py
#andyp 23.11.12

import tkinter

app=tkinter.Tk()
topframe=tkinter.Frame(app)
topframe.pack(side="top")

botframe=tkinter.Frame(app)
botframe.pack(side="bottom")


leftframe=tkinter.Frame(app)
leftframe.pack(side="left")

tplbl=tkinter.Label(leftframe,text = 'label',fg='blue',width="50")
tplbl.pack(side="right")

but1=tkinter.Button(botframe,text = 'button',fg='red')
but1.pack(side="left")

but2=tkinter.Button(botframe,text = 'button',fg='green')
but2.pack(side="left")


tbox1=tkinter.Text(leftframe)
tbox1.insert(1.0,"Who is top of the championship")
tbox1.pack(side="left")


tbox2=tkinter.Text(topframe,fg="blue")
tbox2.insert(1.0,"Who is getting relegated from the championship")
tbox2.pack(side="left")

but3=tkinter.Button(botframe,text = 'button',fg='yellow')
but3.pack(side="bottom")

chBut1=tkinter.Checkbutton(topframe,text="muppet",fg="yellow",bg="black")
chBut1.pack(side="bottom")

chBut2=tkinter.Checkbutton(topframe,text="emu breath",fg="green",bg="yellow")
chBut2.pack(side="top")
app.mainloop()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
