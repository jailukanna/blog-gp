---
title: turtle example dvdPlayerAdvanced (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'dvdPlayerAdvanced'

Functions in program: 
* `def borderCheck():`
* `def moveForegroundY():`
* `def moveForegroundX():`
* `def changeColor():`
* `def moveObject():`

Modules used in program: 
* `import turtle, time, random`

## python dvdPlayerAdvanced

Python turtle example: dvdPlayerAdvanced

```python
# Imports
import turtle, time, random
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


# Constants for Window
SCREEN_WIDTH = 1366
SCREEN_HEIGHT =  768
SCREEN_NAME = "DVD Player"
SCREEN_TITLE = SCREEN_NAME + " (" + str(SCREEN_WIDTH) + "x" + str(SCREEN_HEIGHT) + ")"

# Constants for Log
INFO = "[" + SCREEN_NAME + "/INFO]"
WARN = "[" + SCREEN_NAME + "/WARN]"
ERROR = "[" + SCREEN_NAME + "/ERROR]"
DEBUG = "[" + SCREEN_NAME + "/DEBUG]"

# Debug Mode
debug = False

# Constants for Shape W/H
FG_WIDTH = 128 # Added an additional pixel so the division is on an int rather than a float after division
FG_HEIGHT = 128 # Using a whole nu
FG_WIDTH_SIDE = FG_WIDTH/2
FG_HEIGHT_SIDE = FG_HEIGHT/2

# Foreground Pos Variables
foregroundX = 0
foregroundY = 0
foregroundDirX = 5
foregroundDirY = 5

# Colour Variables | Python is stupid therefore rgb values are between 0-1
red = 0
green = 0
blue = 0

# Set screen window size
screen = turtle.Screen()
screen.setup(SCREEN_WIDTH, SCREEN_HEIGHT)
screen.title(SCREEN_TITLE)
screen.bgcolor((red, green, blue))

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

def changeColor():
    global red, green, blue, screen
    red = random.uniform(0, 1)
    green = random.uniform(0, 1)
    blue = random.uniform(0, 1)
    screen.bgcolor((red, green, blue))
    if debug == True:
        print(INFO, "Changing Colour...")
        print(DEBUG, "Colour: ", round(red, 2), round(green, 2), round(blue, 2))
    else:
        print(INFO, "Changing Colour...")

def moveForegroundX():
    global foregroundDirX
    foregroundDirX = foregroundDirX * -1
    if debug == True:
        print(INFO, "Changing Direction...")
        print(DEBUG, "Foreground X =", foregroundX)
        print(DEBUG, "Foreground Y =", foregroundY)
    else:
        print(INFO, "Changing Direction...")
        
def moveForegroundY():
    global foregroundDirY
    foregroundDirY = foregroundDirY * -1
    if debug == True:
        print(INFO, "Changing Direction...")
        print(DEBUG, "Foreground X =", foregroundX)
        print(DEBUG, "Foreground Y =", foregroundY)
    else:
        print(INFO, "Changing Direction...")

def borderCheck():
    global foregroundX, foregroundY, foregroundDirX, foregroundDirY, screen
    # Right Side Border Detection
    if foregroundX > SCREEN_WIDTH/2 - FG_WIDTH_SIDE:
        moveForegroundX()
        changeColor()
    
    # Left Side Border Detection
    elif foregroundX < ((SCREEN_WIDTH/2 - FG_WIDTH_SIDE) *-1):
        moveForegroundX()
        changeColor()
        
    # Top Side Border Detection
    if foregroundY > SCREEN_HEIGHT/2 - FG_HEIGHT_SIDE:
        moveForegroundY()
        changeColor()
        
    # Bottom Side Border Detection
    elif foregroundY < ((SCREEN_HEIGHT/2 - FG_HEIGHT_SIDE) *-1):
        moveForegroundY()
        changeColor()
        
# Continuious Loop
while True:
    moveObject()
    borderCheck()    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
