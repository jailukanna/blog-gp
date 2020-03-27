---
title: turtle example fullychangeablesquare (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'fullychangeablesquare'


## python fullychangeablesquare

Python turtle example: fullychangeablesquare

```python
from turtle import *
speed("fastest")

for i in range (1):
    ht()
    length= int(input("Please insert the length of your square in actual pixels i.e. 300: "))
    side= int(input("Please insert length of your square i.e. 3: "))
    incpoints= length//side
    x= int(input("rightmost x coordinate: "))
    y= int(input("topmost y coordinate: "))
    w= x
    z= y

    for i in range (1): #the whole shape
        for i in range (side): #side 1
            for i in range (1):   #individual points
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    z=z-(incpoints)
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    w=w-(incpoints)
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    z=z+(incpoints)
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    w=w+(incpoints)
                x=x-(incpoints)  # to change position of main point
        for i in range (side): #side 2
            for i in range (1):   #individual points        
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    z=z-(incpoints)
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    w=w-(incpoints)
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    z=z+(incpoints)
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    w=w+(incpoints)
                y=y-(incpoints)  # to change position of main point
        for i in range (side): #side 3
            for i in range (1):   #individual points        
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    z=z-(incpoints)
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    w=w-(incpoints)
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    z=z+(incpoints)
                for i in range (side):
                    for i in range (1):
                        pu()
                        goto(x,y)
                        pd()
                        goto(w,z)
                    w=w+(incpoints)
                x=x+(incpoints)  # to change position of main point

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
