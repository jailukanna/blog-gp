---
title: tkinter example tkui (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'tkui'

Functions in program: 
* `def make_label(master, x, y, w, h, *args, **kwargs):`
* `def get_value():`
* `def stop_measure():	`
* `def start_measure():`
* `def CloseWindow():`

Modules used in program: 
* `import random`
* `import tkinter.font as font`
* `import tkinter.ttk as ttk`
* `import tkinter as Tk`

## python tkui

Python tkinter example: tkui

```python
#joseph 2017, Jun 7
##Rpi LCD 4'inch 480x320, IPS

import tkinter as Tk
import tkinter.ttk as ttk
import tkinter.font as font
from gpiozero import LED
from threading import Timer
import random



win=Tk.Tk()
win.title("tkinter GUI")
MyFont = font.Font(weight='bold',size=30)

win.attributes('-fullscreen', True) #480x320
global value
def CloseWindow():
    win.destroy()

def start_measure():
    global flag
    global value
    value=0
    flag=1;
    btn_measure.config(state=Tk.DISABLED)
    t = Timer(5.0, stop_measure)
    t.start()
    t1 = Timer(0.2,get_value)
    t1.start()
	
def stop_measure():	
   global flag
   btn_measure.config(state=Tk.NORMAL)
   flag=0   
   
   
def get_value():
    global value
    value=value + random.uniform(-3, 9)   #uniform distribution (a,b)
    label2.config(text=str(round(value,2)) + " mg/dl")
    if flag==1:
        t1 = Timer(0.2,get_value)
        t1.start()
	
   
   
   
	
def make_label(master, x, y, w, h, *args, **kwargs):
    f = Tk.Frame(master, height=h, width=w)
    f.pack_propagate(0) # don't shrink
    f.place(x=x, y=y)
    label = ttk.Label(f, *args, **kwargs)
    label.pack() 
    return label


	
	
label0=make_label(win, 70, 5, 300, 80, text='血糖檢測儀',font = "Helvetica 40 bold italic")
label1=make_label(win, 18, 110, 200, 80, text='血糖:', foreground='red',font = "Helvetica 30 bold italic")
label2=make_label(win, 173, 110, 250, 80, text='0.0 mg/dl', foreground='blue',font = "Helvetica 30 bold italic")
label3=make_label(win, 143, 180, 300, 80, text='啓動中....', foreground='blue',font = "Helvetica 20 bold italic")


#close button to close app
btn_close=ttk.Button(win, text="X", command=CloseWindow)
btn_close.place(x=455,y=5,width=20, height=20)
#display

btn_measure=ttk.Button(win, text="Start",style='TButton', command=start_measure)
btn_measure.place(x=129,y=243,width=0,height=0)  #hidden the start button
ttk.Style().configure('TButton', font=MyFont)


win.after(3000, lambda: label3.config(text='')) # in 3 seconds
win.after(3000, lambda: btn_measure.place(x=129,y=243,width=162,height=63)) # show start button

win.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
