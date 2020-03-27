---
title: turtle example pingturtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'pingturtle'

Functions in program: 
* `def ping(host):`

Modules used in program: 
* `import turtle`
* `import subprocess, os, random, re`

## python pingturtle

Python turtle example: pingturtle

```python
import subprocess, os, random, re
from Tkinter import *
from turtle import *
import turtle

def ping(host):
    process = os.popen("ping -c 1 -n -w 1 "+host, "r")
    output = process.read()
    if "unknown host" in output:
        print(host+" | unknown | 0")
    lines = output.splitlines()
    if len(lines) < 1:
        print(host+" | failure | 0")
        return [0, 0]
    ip = re.sub(".*\(([0-9.]*)\).*\(.*\).*", r"\1", lines[0])
    if len(lines) < 2:
        print(host+" | "+ip+" "*(15-len(ip))+" | 0")
        return [0, 0]
    src = lines[1]
    if "time" not in src:
        print(host+" | "+ip+" "*(15-len(ip))+" | 0")
        return [0, 0]
    else:
        time = re.sub(".*time=(.*) ms", r"\1", src)
        print(host+" | "+ip+" "*(15-len(ip))+" | "+time)
    return [float(time), ip]

# Turtle setup.
setup(width=800, height=800, startx=0, starty=0)
speed("fast")
tracer(True)
colormode(255)
bgcolor((0, 0, 0))

max_ip = 4294967296
chars = "abcdefghijklmnopqrstuvwxyz0123456789"
tlds = [".com", ".org", ".net"]
for a in chars:
    for b in chars:
        for tld in tlds:
            res = ping("www." + "".join([a, b]) + tld)
            if res[0] > 0:
                ip = res[1].split('.');
                num_ip = int(ip[0])*255**3 + int(ip[1])*255**2 + \
                         int(ip[2])*255**1 + int(ip[3])
                rot = (360.0/max_ip) * num_ip
                pencolor((int(ip[0]), int(ip[1]), int(ip[2])))
                right(rot)
                forward(res[0])
                # set back
                penup()
                goto(0, 0)
                pencolor((0, 0, 0))
                pendown()
                setheading(0)
        # save file
        ts = turtle.getscreen()
        ts.getcanvas().postscript(file="out.eps")
done()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
