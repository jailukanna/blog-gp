---
title: turtle example kodeklubben (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'kodeklubben'

Functions in program: 
* `def write(str, w=15):`
* `def plot(str, w):`

## python kodeklubben

Python turtle example: kodeklubben

```python
from turtle import *

def plot(str, w):
    i = 0
    while (i+2 < len(str)):
        p = str[i]
        a = (ord(str[i+1]) - ord('0'))
        d = (ord(str[i+2]) - ord('0'))
        i = i + 3
        if p == 'c':
            pendown()
            circle(d * w, a * 45)
        else:
            if a <= 4:
                right(a * 45)
            else:
                left((8-a) * 45)
            if p == 'u':
                penup()
            elif p == 'd':
                pendown()
            forward(d * w)

speed(5000)
penup()
backward(400)
letters = {
'K': 'd64u42 60c22u40 04 40c22u43 22 60',
'o': 'u01c81u02',
'd': 'u01c81u01d64u44 61',
'e': 'u61d22u60c61d01u01',
'k': 'd64u43 60c21u42u40c21u61u62',
'l': 'd64u44u61',
'u': 'u62d41c41d01u21u22u60',
'b': 'd64u43c81u01u63',
'n': 'd62u22u22d41c41u01u63',
' ': 'u03',
'r': 'd62u22u22u41c41u01u63',
't': 'd64d42d61u01u22u61'
}

def write(str, w=15):
    for i in range(0,len(str)):
        c = str[i]
        if c in letters:
            plot(letters[c], w)
        else:
            plot(letters[' '], w)

write('Kodeklubben er kult')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
