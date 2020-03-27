---
title: turtle example skilpaddetekst (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'skilpaddetekst'

Functions in program: 
* `def linedown(line_len):`
* `def blank():`
* `def backslash(): # \`
* `def slash(): # /`
* `def stick(): # |`
* `def underline(): # _`
* `def _line(length):`

## python skilpaddetekst

Python turtle example: skilpaddetekst

```python
# Author: Ole Kristian Pedersen

from turtle import *
from time import sleep

TEXT = """
______      _   _                                           
| ___ \    | | | |                                          
| |_/ /   _| |_| |__   ___  _ __                            
|  __/ | | | __| '_ \ / _ \| '_ \                           
| |  | |_| | |_| | | | (_) | | | |                          
\_|   \__, |\__|_| |_|\___/|_| |_|                          
       __/ |                                                
      |___/                                                 
                                                            
                                                            
                                                            
                                                            
                                                            
                                                            
                                                            
                                                            
 _   __          _      _    _       _     _                
| | / /         | |    | |  | |     | |   | |               
| |/ /  ___   __| | ___| | _| |_   _| |__ | |__   ___ _ __  
|    \ / _ \ / _` |/ _ \ |/ / | | | | '_ \| '_ \ / _ \ '_ \ 
| |\  \ (_) | (_| |  __/   <| | |_| | |_) | |_) |  __/ | | |
\_| \_/\___/ \__,_|\___|_|\_\_|\__,_|_.__/|_.__/ \___|_| |_|
                                                            
                                                            
 _____                   _ _          _                     
|_   _|                 | | |        (_)                    
  | |_ __ ___  _ __   __| | |__   ___ _ _ __ ___            
  | | '__/ _ \| '_ \ / _` | '_ \ / _ \ | '_ ` _ \           
  | | | | (_) | | | | (_| | | | |  __/ | | | | | |          
  \_/_|  \___/|_| |_|\__,_|_| |_|\___|_|_| |_| |_|          
""".split("\n")

TEXT = [line.rstrip() for line in TEXT]

SIZE = 15

def _line(length):
    pendown()
    forward(length)
    penup()

def underline(): # _
    penup()
    right(90)
    forward(SIZE)
    left(90)
    _line(SIZE)
    left(90)
    forward(SIZE)
    right(90)

def stick(): # |
    penup()
    forward(SIZE/2)
    right(90)
    _line(SIZE)
    left(180)
    forward(SIZE)
    right(90)
    forward(SIZE/2)
    
def slash(): # /
    penup()
    right(90)
    forward(SIZE)
    left(135)
    _line((2*SIZE**2)**0.5)
    right(45)

def backslash(): # \
    right(45)
    _line((2*SIZE**2)**0.5)
    left(135)
    forward(SIZE)
    right(90)

def blank():
    penup()
    forward(SIZE)

def linedown(line_len):
    penup()
    right(90)
    forward(SIZE)
    right(90)
    forward(line_len*SIZE)
    right(180)


MOVES = {
    " " : blank,
    "_" : underline,
    "/" : slash,
    "|" : stick,
    "\\": backslash,
    "(" : stick,        # TODO
    ")" : stick
}

# fullscreen workaround
forward(1) # initialize screen
sleep(3)   # three seconds to select fullscreen
reset()    # force screen redraw

# main loop
while (True):
    shape("turtle")
    speed(11)
    width(5)
    
    # start in upper left corner
    penup()
    setx(-window_width()/2)
    sety(window_height()/2)

    for line in TEXT:
        for c in line:
            MOVES.get(c, blank)()
        linedown(len(line))
    sleep(10)
    reset()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
