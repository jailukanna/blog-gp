---
title: tkinter example open%2520button (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'open%2520button'


Modules used in program: 
* `import tkFileDialog`
* `import Tkinter as Tk`

## python open%2520button

Python tkinter example: open%2520button

```python
import Tkinter as Tk
import tkFileDialog



class Index(object):
    ind = 0

    def next(self, event):
        

    def open(self, event):
        filename = tkFileDialog.askopenfilename() # show an "Open" dialog box and return the path to the selected file
        print(filename)

callback = Index()
axprev = plt.axes([0.7, 0.05, 0.1, 0.075])
axnext = plt.axes([0.81, 0.05, 0.1, 0.075])
bnext = Button(axnext, 'Next')
bnext.on_clicked(callback.next)
bopen = Button(axprev, 'Open')
bopen.on_clicked(callback.open)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
