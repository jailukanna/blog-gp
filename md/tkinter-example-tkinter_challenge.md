---
title: tkinter example tkinter challenge (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'tkinter challenge'


## python tkinter challenge

Python tkinter example: tkinter challenge

```python
"""Write a GUI program to create a simple calculator
Try to be as Pythonic as possible, it's ok if you end up writing
repeated Button and Grid statements, but consider using lists and a for loop
There is no need to store the buttons in variables.

As an optional extra, refer to the documentation to 
work out how to use minsize() to prevent your 
window from being shrunk so that the widgets vanish
from view

Hint: You may want to use the widgets .winfo_height()
winfo_width() methods, in which case you shoul know that
they will not return the correct results unless
the window has been forced to draw the widgets by calling
its .update() method first.

If you are using Windows, you will probably find that 
the width is already constrained and can't be resized 
to small
The height will still need to be constrained, though."""

try:
	import tkinter
except ImportError: #python2
	import Tkinter as tkinter


keys =  [[('C', 1), ('CE',1)],
	    [('7', 1), ('8', 1), ('9', 1), ('+', 1)],	
		[('4', 1), ('5', 1), ('6', 1), ('-', 1)],
		[('1', 1), ('2', 1), ('3', 1), ('*', 1)],
		[('0', 1), ('=', 1), ('/', 1)]
]

#specificam o variabila pt padding:
mainWindowPadding = 8

mainWindow = tkinter.Tk()
mainWindow.title('Calculator')
mainWindow.geometry('640x480-8-200')
mainWindow['padx'] = mainWindowPadding

result = tkinter.Entry(mainWindow)
result.grid(row=0, column = 0, sticky = 'nsew')

keyPad = tkinter.Frame(mainWindow)
keyPad.grid(row=1, column = 0, sticky = 'nsew')

row = 0
for keyRow in keys:
	col = 0
	for key in keyRow:
		tkinter.Button(keyPad, text=key[0]).grid(row=row, column=col, columnspan=key[1], sticky=tkinter.E + tkinter.W) #East, West
		col += key[1]
	row += 1


mainWindow.update()
#asta ca sa nu poti minimiza prea mult fereastra
mainWindow.minsize(keyPad.winfo_width() + mainWindowPadding, result.winfo_height() + keyPad.winfo_height())
#asta ca sa nu poti maximiza prea mult fereastra
mainWindow.maxsize(keyPad.winfo_width() + mainWindowPadding, result.winfo_height() + keyPad.winfo_height())
 
mainWindow.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
