---
title: turtle example L-system (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'L-system'

Functions in program: 
* `def l_system_interpreter(axiom,F,f,left,right,iterations,alpha=90,F_length=20,alphabet={}):`

## python L-system

Python turtle example: L-system

```python
'''
MIT License

Copyright (c) 2018 Dipanshu ggolu2@gmail.com

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
'''

''' 

USAGE:

alphabet= {"X":"X+YF+","Y":"-FX-Y"}
Dragon Curve: l_system_interpreter("FX","F","f","+","-",20,alphabet=alphabet,F_length=4)

'''

from turtle  import *

def l_system_interpreter(axiom,F,f,left,right,iterations,alpha=90,F_length=20,alphabet={}):
    speed(0)
    penup()
    #setpos(-600,-300)
    pendown()
    for i in range(0,iterations):
        string = ""
        for j in axiom:
            if j=="F":
                string += F
            if j=="f":
                string += f
            if j=="+":
                string += left
            if j=="-":
                string += right
            for k in alphabet:
                if j == str(k):
                    string += alphabet[k]
            
        print("L-System at Stage %d : %s" % (i+1,string))
        axiom = string
    
    for j in axiom:
            if j=="F":
                forward(F_length)
            if j=="f":
                penup()
                forward(F_length)
                pendown()
            if j=="+":
                lt(alpha)
            if j=="-":
                rt(alpha)
    hideturtle()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
