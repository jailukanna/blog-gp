---
title: tkinter example frame1 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'frame1'


Modules used in program: 
* `import tkinter`

## python frame1

Python tkinter example: frame1

```python
#frame1.py test gridlayout with use of multiple columns and rows
#andyp 13.03.13

import tkinter

app= tkinter.Tk()
frame=tkinter.Frame(app)
frame.pack()

rightframe=tkinter.Frame(app,width='300')
rightframe.pack(side='right')

txtLabel=tkinter.Label(frame,text='mainframe')
txtLabel.pack()

redbutton = tkinter.Label(rightframe, text="right", fg="red")
redbutton.pack( side = 'top')
greenbutton = tkinter.Label(rightframe, text="right", fg="brown")
greenbutton.pack( side = 'top')
bluebutton = tkinter.Label(rightframe, text="right", fg="blue")
bluebutton.pack( side = 'top' )

leftframe=tkinter.Frame(app)
leftframe.pack(side='bottom')

redbutton1 = tkinter.Button(leftframe, text="Bottom", fg="red")
redbutton1.pack( side = 'left')
greenbutton1 = tkinter.Button(leftframe, text="Bottom", fg="brown")
greenbutton1.pack( side = 'left')
bluebutton1 = tkinter.Button(leftframe, text="Bottom", fg="blue")
bluebutton1.pack( side = 'left' )


app.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
