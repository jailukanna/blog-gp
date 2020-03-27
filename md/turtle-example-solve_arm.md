---
title: turtle example solve arm (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'solve arm'


Modules used in program: 
* `import string`

## python solve arm

Python turtle example: solve arm

```python
from turtle import *
import string

source = open("ev3arm.txt")

inst = []
for line in source:
    p = line[:-2].replace("|", "").replace("  ", " ").split(" ")[2:]
    if len(p) == 0:
        break
    if p[0] == "":
        p = p[2:]
    if p[0][0] not in string.printable:
        p = p[1:]
    obj = {'name': p[0]}
    pp = 1
    while pp < len(p):
        obj[p[pp][:-1]]=p[pp+1]
        pp += 2
    inst.append(obj)
print(inst)

showturtle()
seth(180)
up()
fd(700)
for obj in inst:
    if "port_motor" in obj:
        if obj["port_motor"] == "B":
            if int(obj["speed"]) < 0:
                down()
            elif int(obj["speed"]) > 0:
                up()
        elif obj["port_motor"] == "A":
            if int(obj["speed"]) < 0:
                left(90)
                fd(int(obj["rotations"])/10)
                right(90)
            elif int(obj["speed"]) > 0:
                right(90)
                fd(int(obj["rotations"])/10)
                left(90)
        elif obj["port_motor"] == "C":
            if int(obj["speed"]) > 0:
                bk(float(obj["rotations"])*10)
            elif int(obj["speed"]) < 0:
                fd(float(obj["rotations"])*10)

raw_input()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
