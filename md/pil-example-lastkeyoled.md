---
title: pil example lastkeyoled (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'lastkeyoled'


Modules used in program: 
* `import termios`
* `import sys`
* `import struct`
* `import Adafruit_SSD1306`

## python lastkeyoled

Python pil example: lastkeyoled

```python
import Adafruit_SSD1306
import struct
import sys
import termios
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

# 128x64 display with hardware I2C:
disp = Adafruit_SSD1306.SSD1306_128_64(rst=24)

disp.begin()
width = disp.width
height = disp.height
disp.clear()
disp.display()

textimage = Image.new('1', (height, width))
textdraw = ImageDraw.Draw(textimage)

# Make sure the .ttf font file is in the same directory as this python script!
# Get it off Google Fonts.
font = ImageFont.truetype('VT323-Regular.ttf', 100)
fmt = '2IHHI'  # On 64-bit, it's 4IHHI instead (bigger timestamp at front)
EV_KEY = 1
VAL_KEYDOWN, VAL_KEYUP = 1, 0
KEYS = {
 2: '1', 3: '2', 4: '3', 5: '4', 6: '5', 7: '6', 8: '7', 9: '8', 10: '9',
 11: '0', 12: '-', 13: '=',

 16: 'q', 17: 'w', 18: 'e', 19: 'r', 20: 't', 21: 'y', 22: 'u', 23: 'i',
 24: 'o', 25: 'p', 26: '[', 27: ']', 43: '\\',

 30: 'a', 31: 's', 32: 'd', 33: 'f', 34: 'g', 35: 'h', 36: 'j', 37: 'k',
 38: 'l', 39: ';', 40: '\'',

 44: 'z', 45: 'x', 46: 'c', 47: 'v', 48: 'b', 49: 'n', 50: 'm', 51: ',',
 52: '.', 53: '/',
}

f = open('/dev/input/event1', 'rb')
while True:
    data = f.read(struct.calcsize(fmt))
    t0, t1, evtype, code, val = struct.unpack(fmt, data)
    if evtype != EV_KEY: continue
    if val != VAL_KEYDOWN: continue
    #print('keydown' + KEYS.get(code, 'not found %d' % code))
    if code not in KEYS: continue

    textdraw.rectangle((0,0,height,width), outline=0, fill=0)
    textdraw.text((0, 0), KEYS[code].upper(), font=font, fill=255)
    disp.image(textimage.transpose(Image.ROTATE_90))
    disp.display() 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
