---
title: turtle example dvdPlayer (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'dvdPlayer'

Functions in program: 
* `def borderCheck():`
* `def moveObject():`

Modules used in program: 
* `import turtle, time`

## python dvdPlayer

Python turtle example: dvdPlayer

```python
# Imports
import turtle, time
from turtle import *

# Depreciated Code
# ================
# SCREEN_TITLE = input("Screen Title: ")
# SCREEN_WIDTH = input("ScreenWidth: ")
# SCREEN_HEIGHT = input("ScrenHeight: ")
#if SCREEN_WIDTH == "" and SCREEN_HEIGHT == "" and SCREEN_TITLE == "":
#    SCREEN_WIDTH = 640
#    SCREEN_HEIGHT = 480
#    SCREEN_TITLE = "dvdPlayer.py"
#else:
#    SCREEN_WIDTH = int(SCREEN_WIDTH)
#    SCREEN_HEIGHT = int(SCREEN_HEIGHT)
#    SCREEN_TITLE = SCREEN_TITLE
#
# ================
#

# Constants for Window
SCREEN_WIDTH = 1229
SCREEN_HEIGHT =  768
SCREEN_TITLE = "DVD Player " + "(" + str(SCREEN_WIDTH) + "x" + str(SCREEN_HEIGHT) + ")"

# Constants for Shape W/H
FG_WIDTH = 220 # Added an additional pixel so the division is on an int rather than a float after division
FG_HEIGHT = 200 # Added an additional pixel so the division is on an int rather than a float after division
FG_WIDTH_SIDE = FG_WIDTH/2
FG_HEIGHT_SIDE = FG_HEIGHT/2
    
# Foreground Pos Variables
foregroundX = 0
foregroundY = 0
foregroundDirX = 5
foregroundDirY = 5

# Set screen window size
screen = turtle.Screen()
screen.setup(SCREEN_WIDTH, SCREEN_HEIGHT)
screen.title(SCREEN_TITLE)
screen.bgpic("assets/background.gif")

# Define object turtle as foreground then specify properties of the object (Object Oreninted Programming)
foreground = turtle.Turtle()
register_shape("assets/foreground.gif")
foreground.shape("assets/foreground.gif")
foreground.penup()

def moveObject():
    global foregroundX, foregroundY, foregroundDirX, foregroundDirY, screen
    foreground.setpos(foregroundX, foregroundY)
    time.sleep(0.01)

    foregroundX = foregroundX + foregroundDirX
    foregroundY = foregroundY + foregroundDirY

def borderCheck():
    global foregroundX, foregroundY, foregroundDirX, foregroundDirY, screen
    # Right Side Border Detection
    if foregroundX > SCREEN_WIDTH/2 - FG_WIDTH_SIDE:
        foregroundDirX = foregroundDirX * -1

    # Left Side Border Detection
    elif foregroundX < ((SCREEN_WIDTH/2)) * -1 + FG_WIDTH_SIDE:
        foregroundDirX = foregroundDirX + 1

    # Top Side Border Detection
    if foregroundY > SCREEN_HEIGHT/2 - FG_HEIGHT_SIDE:
        foregroundDirY = foregroundDirY * -1

    # Bottom Side Border Detection
    elif foregroundY < ((SCREEN_HEIGHT/2)) * -1 + FG_HEIGHT_SIDE:
        foregroundDirY = foregroundDirY + 1

# Continuious Loop
while True:
    moveObject()
    borderCheck()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
