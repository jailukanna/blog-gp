---
title: turtle example buggy (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'buggy'

Functions in program: 
* `def bumpers():`
* `def done():`
* `def backward(distance):`
* `def forward(distance):`
* `def left(angle):`
* `def right(angle):`
* `def move(left,right,steps):`
* `def penup():`
* `def pendown():`

Modules used in program: 
* `import time`

## python buggy

Python turtle example: buggy

```python
from ABElectronics_IOPi import IOPI
import time

# address 0x20 is IC1, 0x21 is IC2
ic2 = IOPI(0x21)

fr1 = 1
index = 2
fr2 = 3
motors = 4
pen_ud = 5
tilt = 6
rhs = 7
lhs = 8

delay = 0.01
distance_scale = 0.7
angular_scale = 1.02

fwd = 0
rev = 1
on = 0
off = 1

# Set pins PB0 - PB7
# Port 0 controls pins 1 to 8, port 1 controls pins 9 to 16
# 0 is output, 1 is input
ic2.setPortDirection(0, 0xE0)

# Turn off all pins
# pen up, motors off
ic2.writePort(0, 0x08)

def pendown():
    print("pendown")
    ic2.writePin(pen_ud,1)

def penup():
    print("penup")
    ic2.writePin(pen_ud,0)

def move(left,right,steps):
    ic2.writePin(motors,on)
    ic2.writePin(fr1,right)
    ic2.writePin(fr2,left)
    for i in range(int(steps)):
        ic2.writePin(index,0)
        time.sleep(delay)
        ic2.writePin(index,1)
        time.sleep(delay)

def right(angle):
    print("right",angle)
    if angle>=0:
        move(fwd,rev,angle*angular_scale)
    else:
        move(rev,fwd,-angle*angular_scale)

def left(angle):
    print("left",angle)
    if angle>=0:
        move(rev,fwd,angle*angular_scale)
    else:
        move(fwd,rev,-angle*angular_scale)

def forward(distance):
    print("forward",distance)
    if distance>=0:
        move(fwd,fwd,distance*distance_scale)
    else:
        move(rev,rev,-distance*distance_scale)

def backward(distance):
    print("backward",distance)
    if distance>=0:
        move(rev,rev,distance*distance_scale)
    else:
        move(fwd,fwd,-distance*distance_scale)

def done():
    pendown()
    # motors are active low    
    ic2.writePin(motors,off)
    print("done")

def bumpers():
	return [ic2.readPin(lhs), ic2.readPin(rhs)]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
