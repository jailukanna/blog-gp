---
title: turtle example csci1133-exercises-01 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'csci1133-exercises-01'

Functions in program: 
* `def testSlope():`
* `def BMR():`
* `def calculateBMR(gender, weight, height, age):`
* `def findLine():`
* `def intercept(line_slope, x, y):`
* `def slope(x1, y1, x2, y2):`
* `def drawStar():`
* `def computeWC():`
* `def windchill(temp, wind):`

Modules used in program: 
* `import math`

## python csci1133-exercises-01

Python turtle example: csci1133-exercises-01

```python

from turtle import *
import math

def windchill(temp, wind):
    wc=35.74 + 0.6215*temp - 35.75 * wind ** 0.16 +0.4275 * temp * wind**0.16
    return wc

def computeWC():
    t = float(input("enter temperature (F): "))
    w = float(input("enter wind velocity (MPH): "))
    if t in range(-50, 120) and w in range(0, 200):
        print("the windchill is " + str(windchill(t, w)))
    else:
        print("the temperature or range is out of bounds")

def drawStar():
    length = int(input("enter the length of each side (less than 500, but greater than 0: "))
    if length in range(0, 500):
        for i in range(0, 5):
            forward(length)
            right(216)
    else:
        print("the entered length is too big")
        
def slope(x1, y1, x2, y2):
    if x1 == x2:
        return math.inf
    else:
        return (y2-y1)/(x2-x1)

def intercept(line_slope, x, y):
    return y - x * line_slope

def findLine():
    """
    x1 = float(input("enter first x value: "))
    y1 = float(input("enter first y value: "))
    x2 = float(input("enter second x value: "))
    y2 = float(input("enter second y value:  "))"""
    p1 = input("enter the x and y values for the first point").split()
    p2 = input("enter the x and y values for the second point").split()
    #m = slope(x1, y1, x2, y2)
    #b = intercept(m, x1, y1)
    if p1[0] == p2[0]:
        m = math.inf
    else:
        m = slope(float(p1[0]), float(p1[1]), float(p2[0]), float(p2[1]))
    b = intercept(m, float(p1[0]), float(p1[1]))
    print("y = ", m, "x + ", b)

def calculateBMR(gender, weight, height, age):
    if gender == "m":
        return 655 + (4.3 * weight)+(4.7 * height)-(4.7*age)
    elif gender == "f":
        return 66 + (6.3 * weight) + (12.9 * height) - (6.8 * age)
    
def BMR():
    gender = input("enter your gender (m/f, anything else will not work)")
    #gender = lower(gender)
    age = int(input("enter your age in years"))
    weight = int(input("enter your weight in pounds"))
    height = int(input("enter your height in inches"))
    return calculateBMR(gender, weight, height, age)

"""used as a unit test for the slope function"""

def testSlope():
    print(slope(0, 2, -1, 20))
    print(slope(10, 20, 10, -40))
    print(slope(-29, 10, -39, 0))
    print(slope(0, 0, 0, 0))
    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
