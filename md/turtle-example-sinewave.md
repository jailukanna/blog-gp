---
title: turtle example sinewave (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'sinewave'


Modules used in program: 
* `import math`

## python sinewave

Python turtle example: sinewave

```python
   
import math

penup()
goto(-500,0)
pendown()
for x in range(-500,500,4):
    y = math.sin(x/50) * 100
    goto(x,y)
    
    
    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
