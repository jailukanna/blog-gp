---
title: turtle example passtest (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'passtest'

Functions in program: 
* `def sbt(sifre):`
* `def gosterge(uzunluk,b):`

Modules used in program: 
* `import time `
* `import re`

## python passtest

Python turtle example: passtest

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import re
from turtle import *
import time 

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

parola=raw_input("şifreyi giriniz: ")


a='.*[\[,/,^,+,%,&,/,(,),=,?,_,>,:,;,},{.,<,|,`,\],<,-]'
def sbt(sifre):
    
    global seviye
    
    seviye=0
    
    if re.search("[0-9]",sifre):
        seviye=seviye+1

    if re.search("[a-z]",sifre):
        seviye=seviye+1

    if re.search("[A-Z]",sifre):
        seviye=seviye+1
        
    if re.search(a,sifre) :
        seviye=seviye+1

sbt(parola)

if seviye == 0 :
    print"klavyeye dokunmayı dene"

    gosterge(6,0)
    
elif seviye == 1 : 
    print( "zayıf")

    gosterge(10,1)    

elif seviye == 2 :
    print("orta")

    gosterge(14,3)

elif seviye == 3 :
    print"güçlü"
    
    gosterge(18,4)
else:
    print"çok güçlü"
    
    gosterge(22,5)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
