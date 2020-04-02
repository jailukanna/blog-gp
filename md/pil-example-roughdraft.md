---
title: pil example roughdraft (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'roughdraft'

Functions in program: 
* `def beep(x):`
* `def off():`
* `def on():`
* `def c_to_f(c):`

Modules used in program: 
* `import Adafruit_MAX31855.MAX31855 as MAX31855`
* `import Adafruit_SSD1306`
* `import Adafruit_GPIO.SPI as SPI`
* `import math`
* `import time`

## python roughdraft

Python pil example: roughdraft

```python
import time
import math
from datetime import datetime
import Adafruit_GPIO.SPI as SPI
import Adafruit_SSD1306
from dateutil import relativedelta
from datetime import timedelta
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont
from squid import *
import Adafruit_MAX31855.MAX31855 as MAX31855


# Define a function to convert celsius to fahrenheit.
def c_to_f(c):
        return c * 9.0 / 5.0 + 32.0

# Raspberry Pi pin configuration:
RST = None     # on the PiOLED this pin isnt used
# Note the following are only used with SPI:
DC = 23
SPI_PORT = 0
SPI_DEVICE = 0
rgb = Squid(16, 20, 21) 

# Raspberry Pi software SPI configuration.
CLK = 11
CS  = 9
DO  = 10
sensor = MAX31855.MAX31855(CLK, CS, DO)

# 128x32 display with hardware I2C:
disp = Adafruit_SSD1306.SSD1306_128_32(rst=RST)

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

# Draw some shapes.
# First define some constants to allow easy resizing of shapes.
padding = -2
top = padding
bottom = height-padding
# Move left to right keeping track of the current x position for drawing shapes.
x = 0


# Load default font.
font = ImageFont.load_default()

# Alternatively load a TTF font.  Make sure the .ttf font file is in the same directory as the python script!
# Some other nice fonts to try: http://www.dafont.com/bitmap.php
# font = ImageFont.truetype('Minecraftia.ttf', 8)

BuzzerPin = 17

GPIO.setup(BuzzerPin, GPIO.OUT)
GPIO.output(BuzzerPin, GPIO.HIGH)

#turn buzzer on
def on():
    GPIO.output(BuzzerPin, GPIO.LOW)

#turn buzzer off
def off():
    GPIO.output(BuzzerPin, GPIO.HIGH)

#beep sequence
def beep(x):
    on()
    time.sleep(x)
    off()
    time.sleep(x)

decarbThreshold = 75
decarbDuration = 1
aboveThreshold = False
decarbEndTime = None
temperatureAlarm = False
start = datetime.now()

while True:

#    temp = sensor.readTempC()
    temp = sensor.readInternalC()
    if math.isnan(temp):
        continue
    print("F: "+str(c_to_f(temp)))
    #when temp goes over threshold set start time and end time
    if(c_to_f(temp) > decarbThreshold and aboveThreshold == False):
        aboveThreshold = True
        decarbStartTime = datetime.now()
        decarbEndTime = datetime.now()+timedelta(minutes=decarbDuration)
    #alarm if temp goes back under threshold set alarm to true
    if(c_to_f(temp) < decarbThreshold and aboveThreshold == True):
        temperatureAlarm = True
    #get current time and calculate elapsed time
    now = datetime.now()
    difference = relativedelta.relativedelta(now, start)
    #calculate remaining decarb time
    decarbTime = relativedelta.relativedelta(decarbEndTime, now)
    #print(time left/elapsed time)
    print("DECARB: "+str(decarbTime))
    print("time left (H:M:S): "+str(decarbTime.hours)+":"+str(decarbTime.minutes)+":"+str(decarbTime.seconds))
    print("elapsed   (H:M:S): "+str(difference.hours)+":"+str(difference.minutes)+":"+str(difference.seconds))
    # Draw a black filled box to clear the image.
    draw.rectangle((0,0,width,height), outline=0, fill=0)
    draw.text((x, top),       "Decarboxilator",  font=font, fill=255)
    draw.text((x, top+8),     "Current Temp: "+str(c_to_f(temp)), font=font, fill=255)
    draw.text((x, top+16),    "Elapsed Time: "+str(difference.hours)+":"+str(difference.minutes)+":"+str(difference.seconds),  font=font, fill=255)
    if(decarbTime.hours == 0 and decarbTime.minutes == 0 and decarbTime.seconds == 0):
        draw.text((x, top+25),    "Threshold not reached ", font=font, fill=255)
    else:
        draw.text((x, top+25),    "Time Left: "+str(decarbTime.hours)+":"+str(decarbTime.minutes)+":"+str(decarbTime.seconds),  font=font, fill=255)
    if(decarbEndTime != None and decarbEndTime < datetime.now()):
       rgb.set_color(GREEN)
       #turn on buzzer
       on()
    if(aboveThreshold == True):
       rgb.set_color(BLUE)
    if(temperatureAlarm == True):
       rgb.set_color(RED)
       #turn on buzzer
       on()
    # Display image.
    disp.image(image)
    disp.display()
    time.sleep(.1)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
