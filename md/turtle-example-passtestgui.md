---
title: turtle example passtestgui (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'passtestgui'

Functions in program: 
* `def onay():`
* `def sbt(sifre):`
* `def gosterge(uzunluk,b):`

Modules used in program: 
* `import time `
* `import re`

## python passtestgui

Python turtle example: passtestgui

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
from Tkinter import *
import re
from turtle import *
import time 



pen = Tk()
pen.geometry("200x200+10+10")
pen.title("Şifre Test")

yazi = Label(text="Şifrenizi girin:")
yazi.place(relx=0.3, rely=0.1)

giris = Entry()
giris.place(relx= 0.1, rely=0.2)

yazi1=Label()
yazi1.place(relx=0.1, rely=0.7)

def gosterge(uzunluk,b):
    renkler=["#ffffc0","#ffc000","#ff8000","#ff4000","#ff0000"]
    pensize(20)
    a=0
    for i in range(b):
        renk=renkler[a]
        pencolor(renk)
        forward(uzunluk)
        a=a+1
    time.sleep(5)




a='.*[\[,/,^,+,%,&,/,(,),=,?,_,>,:,;,},{.,<,|,`,\],<,-]'

       
def sbt(sifre):


    seviye=0
    
    if re.search("[0-9]",sifre):
        seviye=seviye+1

    if re.search("[a-z]",sifre):
        seviye=seviye+1

    if re.search("[A-Z]",sifre):
        seviye=seviye+1
        
    if re.search(a,sifre) :
        seviye=seviye+1
    
    if seviye == 0 :
	
        yazi1["text"]="şifre boş bırakılamaz!"
	gosterge(6,0)
    
    elif seviye == 1 : 
	
        yazi1["text"]="Şifreniz zayıf seviyede"
	gosterge(10,1)    
	
    elif seviye == 2 :
	
        yazi1["text"]="Şifreniz orta seviyede"
	gosterge(14,3)

    elif seviye == 3 :
	
        yazi1["text"]="Şifreniz güçlü seviyede"
	gosterge(18,4)	
    else:
	
	yazi1["text"]="Şifreniz çok güçlü seviyede"	
	gosterge(22,5)

def onay():
    
    sbt(giris.get())   


btn = Button(text="degerlendir", command=onay)
btn.place(relx=0.25, rely=0.4)
    
mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
