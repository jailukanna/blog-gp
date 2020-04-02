---
title: pil example animate (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'animate'


Modules used in program: 
* `import RPi.GPIO as GPIO`
* `import Adafruit_SSD1306`
* `import Adafruit_GPIO.SPI as SPI`
* `import random`
* `import time`
* `import math`

## python animate

Python pil example: animate

```python
import math
import time
import random

import Adafruit_GPIO.SPI as SPI
import Adafruit_SSD1306

from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

import RPi.GPIO as GPIO

#import logging
#logging.basicConfig(level=logging.DEBUG)

class LEDS:
    def __init__(self):
        #LED_PINS = [11, 12, 13, 15, 16, 22, 29, 31, 32]
        self.PINS = [17, 18, 27, 22, 23, 25, 5, 6, 12]
        GPIO.setmode(GPIO.BCM)
        GPIO.setup(self.PINS, GPIO.OUT)
        self.state = 0

    def random_pattern(self):
        for n in self.PINS:
            GPIO.output(n, random.randint(0,1))

class OLED:
    def __init__(self):
        self.RESET_PIN = 24
        self.disp = Adafruit_SSD1306.SSD1306_128_32(rst=self.RESET_PIN)
        self.font = ImageFont.truetype('c.ttf', 32)
        self.disp.begin()
        self.disp.clear()
        self.disp.display()
        self.setText('ERROR! Text not set!')
        self.textpos = 0

    def setText(self, text):
        image = Image.new('1', (self.disp.width, self.disp.height))
        draw = ImageDraw.Draw(image)
        self.maxwidth, unused = draw.textsize(text, font=self.font)
        self.FullTextImage = Image.new('1', (self.maxwidth+2*self.disp.width, self.disp.height))
        self.FTIdraw = ImageDraw.Draw(self.FullTextImage)
        self.FTIdraw.text((self.disp.width, 0), text, font=self.font, fill=255)

    def animate(self, speed=2):
        section = self.FullTextImage.crop((self.textpos, 0, self.textpos+self.disp.width, self.disp.height))
        self.disp.image(section)
        self.disp.display()
        self.textpos = self.textpos+speed
        if self.textpos>(self.maxwidth+self.disp.width):
            self.textpos = 0

####### MAIN ###########

leds = LEDS()
disp = OLED()
disp.setText('Ich bin ROBOTER <add name here>’)

counter = 0
while True:
    tstamp = time.time()
    disp.animate()
    if counter>9:
        leds.random_pattern()
        counter = 0
    counter = counter+1
    sleeptime = 0.01 - (time.time()-tstamp)
    if sleeptime>0:
        time.sleep(sleeptime)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
