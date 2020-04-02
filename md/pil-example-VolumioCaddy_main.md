---
title: pil example VolumioCaddy main (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'VolumioCaddy main'

Functions in program: 
* `def SoundOut(Mute):`
* `def Volume(level):`
* `def Repeat(singleR, mulitpleR):`
* `def Random(RandomState):`
* `def Disp_Symbols(Status):`

Modules used in program: 
* `import subprocess`
* `import Adafruit_SSD1306`
* `import Adafruit_GPIO.SPI as SPI`
* `import  RPi.GPIO as GPIO`
* `import os`
* `import time`

## python VolumioCaddy main

Python pil example: VolumioCaddy main

```python
# Author : Julien Croteau 
# Date : 09/01/2018
# Version : 0.0.3 

import time
import os
import  RPi.GPIO as GPIO

import Adafruit_GPIO.SPI as SPI
import Adafruit_SSD1306

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

from buttons import Buttons, Rotary

import subprocess

# Sets GPIO mode
GPIO.setmode(GPIO.BCM)

# declaring button instances           
play = Buttons( 22, "toggle", "sudo shutdown now")
nxt = Buttons( 27, "next", False)
prev = Buttons( 17, "previous", False)
vlm = Rotary(25, "setRandom(true|false)", "volumio volume mute", 23, 24) # This one is for the rotary encoder

# Raspberry Pi pin configuration:
RST = 26     # on the PiOLED this pin isnt used

# 128x64 display with hardware I2C:
disp = Adafruit_SSD1306.SSD1306_128_64(rst=RST)

# Initialize library.
disp.begin()

# Clear display.
disp.clear()
disp.display()

# Create blank image for drawing.
# Make sure to create image with mode '1' for 1-bit color.
width = disp.width
height = disp.height
image = Image.new('1', (width, height))

# Get drawing object to draw on image.
draw = ImageDraw.Draw(image)

# Draw a black filled box to clear the image.
draw.rectangle((0,0,width,height), outline=0, fill=0)

# First define some constants to allow easy resizing of shapes.
padding = -2
top = padding
bottom = height-padding
# Move left to right keeping track of the current x position for drawing shapes.
x = 0

# Find the directory of the file
path = os.getcwd()

# Load fonts.
# The .ttf files must be in the same directory as this script!
font = ImageFont.truetype(path+'/ProggyClean.ttf', 16)
font2 = ImageFont.truetype(path+'/InkyThinPixels.ttf', 16)
symbols = ImageFont.truetype(path+'/VolumioOLED.ttf', 16) # special Icon font

# Replaces play and pause with the corresponding letter of the symbol font
def Disp_Symbols(Status):
        if (Status == "pause"):
                return "B"
        elif (Status == "play"):
                return "A"
        else:
                return "E"
            
# To display is ramndom is activated with symbol font.
def Random(RandomState):
    if RandomState:
        return "J"
    else:
        return "O"
    
# To display if repeat or single repeat is on with symbol font.
def Repeat(singleR, mulitpleR):
    if singleR:
        return "H"
    elif multipleR:
        return "I"
    else:
        return "O"
    
# To display volume proprely
def Volume(level):
    if level == 0:
        return "--"
    else:
        return str(level)
    
# To display the volume Icon
def SoundOut(Mute):
    if Mute != 0:
        return "G"
    else:
        return "F"
try:
    while True:

       # Draw a black filled box to clear the image.
       draw.rectangle((0,0,width,height), outline=0, fill=0)

       # Takes in Volumio status
       status = subprocess.check_output("volumio status | sed -e 's/true/True/g' -e 's/false/False/g' -e 's/null/False/g'", shell=True)
       status = eval(status)
       # Assigining the result to variables with default values
       try:
           State = status.get("status")
       except:
           State = "Oops"
       try:
           RandomState = status.get("random")
       except:
           RandomState = False
       try:
           singleR = status.get("repeatSingle")
       except:
           singleR = False
       try:
           multipleR = status.get("repeat")
       except:
           multipleR = False
       try:
           level = status.get("volume")
       except:
            level = 0
       try:
           Mute = status.get("mute")
       except:
           Mute = True
       try:
           Artist = str(status.get("artist"))
       except:
           Artist = "J'aime la saveur du"
       try:
           Title = str(status.get("title"))
       except:
           Title = "Baloney"
       try:
           SampleRate = status.get("samplerate")
       except:
           SampleRate = "Send"
       try:
           BitDepth = status.get("bitdepth")
       except:
           BitDepth = "nudes"
       # Status bar constants
       Seek = status["seek"]
       Duration = status["duration"]
       SeekNormalized = int(Seek) / 1000
       try :
           BarWidth = SeekNormalized * 128 / int(Duration) 
       except :
           BarWidth = 1
       # Write lines of text.
       draw.text((x+1 , top),        Disp_Symbols(State),  font=symbols, fill=255)
       draw.text((x+30, top),       Random(RandomState), font=symbols, fill=255)
       draw.text((x+60, top),      Repeat(singleR, multipleR), font=symbols, fill=255)
       draw.text((x+93, top),      SoundOut(Mute), font=symbols, fill=255)
       draw.text((x+105, top+2),      Volume(level), font=font, fill=255)
       draw.text((x, top+15),     Artist, font=font, fill=255)
       draw.text((x, top+29),    Title,  font=font2, fill=255)
       draw.text((x, top+48),   str(SampleRate) + " " + str(BitDepth),  font=font, fill=255)
       # Draw Status Bar
       draw.rectangle(( x, 62, x+BarWidth, 64 ), outline=255, fill=255) 
       # Display image.
       disp.image(image)
       disp.display()
       time.sleep(.1)
        
except: # clears the display when program is exited
    disp.clear()
    disp.display()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
