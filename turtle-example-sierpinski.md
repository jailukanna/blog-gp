---
title: turtle example sierpinski (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'sierpinski'

Functions in program: 
* `def sierpinski(n, lados):`

## python sierpinski

Python turtle example: sierpinski

```python
from turtle import *
 
def sierpinski(n, lados):
  if n == 0:
   	 for i in range(3):
   		 fd(lados)   	 
   		 left(120)
        
  else: #recursividade
   	 sierpinski(n-1, lados/2.0)
   	 fd(lados/2.0)
   	 sierpinski(n-1, lados/2.0)
   	 bk(lados/2.0)
   	 left(60)
   	 fd(lados/2.0)
   	 right(60)
   	 sierpinski(n-1, lados/2.0)
   	 left(60)
   	 bk(lados/2.0)
   	 right(60)
   	 

shape("turtle")
speed(0) #fast n furious
sierpinski(6,700)
exitonclick()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
