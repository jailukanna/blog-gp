---
title: tkinter example tkinter (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'tkinter'


Modules used in program: 
* `import tkinter`
* `import tkinter`
* `import tkinter`

## python tkinter

Python tkinter example: tkinter

```python
import tkinter
print(tkinter.TkVersion) #astea 2 verifica versiunea
print(tkinter.TclVersion)
#tkinter._test() #face sa apara o fereastra grafica
mainWindow = tkinter.Tk()
mainWindow.title("Hello WOrld!")
mainWindow.geometry("640x480+8+400") #aici e string, nu numar
	#8px e distanta fata de marginea din stanga a ecranului
	#400 e distanta fata de marginea din dreapta a ecr
mainWindow.mainloop() #cand rulezi asta predai controlul lui tkinter
"""daca vrei sa faci programul compatibil si cu python2, faci asta
try: 
	import tkinter
except ImportError: #python2
	importTkinter as tkinter """

import tkinter

print(tkinter.TkVersion) #astea 2 verifica versiunea
print(tkinter.TclVersion)
#tkinter._test() #face sa apara o fereastra grafica
mainWindow = tkinter.Tk()
mainWindow.title("Hello WOrld!")
mainWindow.geometry("640x480+8+400") #aici e string, nu numar
	#8px e distanta fata de marginea din stanga a ecranului
	#400 e distanta fata de marginea din dreapta a ecr

#intai cream widgetul 'label'
label = tkinter.Label(mainWindow, text = "Fear is the mind-killer!")
label.pack(side="top")

leftFrame =tkinter.Frame(mainWindow)
leftFrame.pack(side='left', anchor='n', fill =tkinter.Y, expand=False) #n=nord

#face ca chenarul sa fie in relief
#acum folosim leftFrame in loc de mainWindow
canvas = tkinter.Canvas(leftFrame, relief = 'raised', borderwidth =1,)
#putem face ca chenarul sa umple fereastra in care se afla cu fill
#nu umple si spatiu folosit pt labelul de mai sus
canvas.pack(side="left", anchor='n') # fill=tkinter.Y, expand =True 
#Y e axa y si extinde pe verticala chenarul
#expand =True extinde canvasul pe orizontala

rightFrame = tkinter.Frame(mainWindow)
rightFrame.pack(side='right', anchor='n', expand=True)


#butoanele nu mai folosesc mainWindow, ci rightFrame
button1 = tkinter.Button(rightFrame, text="button1")
button2 = tkinter.Button(rightFrame, text="button2")
button3 = tkinter.Button(rightFrame, text="button3")

#pozitiile de aici nord sud etc sunt afectate si de side='top'
#de mai sus, de la canvas.pack;
button1.pack(side="top")
button2.pack(side="top")
button3.pack(side="top")

mainWindow.mainloop() #cand rulezi asta predai controlul lui tkinter
"""daca vrei sa faci programul compatibil si cu python2, faci asta
try: 
	import tkinter
except ImportError: #python2
	importTkinter as tkinter """

#PACK FOLOSESTE POZITIE RELATIVA
#PLACE fol poz ABSOLUTA, pt cel putin o fereastra
#iar daca nu sti exact marimea ecranului, nu e recomandabil
#sa folosesti pozitie absoluta


ex2:
#acum folosim grid pe unde aveam inainte pack
import tkinter

print(tkinter.TkVersion) #astea 2 verifica versiunea
print(tkinter.TclVersion)
#tkinter._test() #face sa apara o fereastra grafica
mainWindow = tkinter.Tk()
mainWindow.title("Hello WOrld!")
mainWindow.geometry("640x480-8-200") #aici e string, nu numar
	#8px e distanta fata de marginea din stanga a ecranului
	#200 e distanta fata de marginea din dreapta a ecr

#intai cream widgetul 'label'
label = tkinter.Label(mainWindow, text = "Fear is the mind-killer!")
label.grid(row=0, column=0)

leftFrame =tkinter.Frame(mainWindow)
leftFrame.grid(row=1, column=1)

#face ca chenarul sa fie in relief
#acum folosim leftFrame in loc de mainWindow
canvas = tkinter.Canvas(leftFrame, relief = 'raised', borderwidth =1,)
canvas.grid(row=1, column=0) 

#proprietatea STICKY face cam ce face anchor pt pack
#e un fel de busola
rightFrame = tkinter.Frame(mainWindow)
rightFrame.grid(row=1, column=2, sticky='n') #n=nord (butoanele la nord)


#butoanele nu mai folosesc mainWindow, ci rightFrame
button1 = tkinter.Button(rightFrame, text="button1")
button2 = tkinter.Button(rightFrame, text="button2")
button3 = tkinter.Button(rightFrame, text="button3")

#pozitiile de aici nord sud etc sunt afectate si de side='top'
#de mai sus, de la canvas.pack;
button1.grid(row=0, column=0)
button2.grid(row=1, column=0)
button3.grid(row=2, column=0)

#configure the columns
#the weight options is important in deciding the behaviour of the widgets within a cell
#unless a weight is set for a column or row, some options, including sticky, won't do anything
#untill a WEIGHT has being set,  the colum has not been
#sized to fit a window.
mainWindow.columnconfigure(0, weight=1) #coloana 0
mainWindow.columnconfigure(1, weight=1) #coloana 1
mainWindow.grid_columnconfigure(2, weight=1)

leftFrame.config(relief= 'sunken', borderwidth=1)
rightFrame.config(relief='sunken', borderwidth=1)
leftFrame.grid(sticky='ns') 
rightFrame.grid(sticky='new') #nord, est, west

rightFrame.columnconfigure(0, weight=1)
button2.grid(sticky='ew') #a intins butonu2 in tot frameul 2, de la est la vest doar
mainWindow.mainloop() #cand rulezi asta predai controlul lui tkinter



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
