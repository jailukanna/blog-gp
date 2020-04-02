---
title: PIL example btc wallet scroller (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'btc wallet scroller'


Modules used in program: 
* `import Adafruit_SSD1306`
* `import Adafruit_GPIO.SPI as SPI`
* `import requests`
* `import json`
* `import time`

## python btc wallet scroller

Python PIL example: btc wallet scroller

```python
#!/usr/bin/python3

import time
import json
import requests
import Adafruit_GPIO.SPI as SPI
import Adafruit_SSD1306

from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

# Raspberry Pi pin configuration:
RST = None
disp = Adafruit_SSD1306.SSD1306_128_32(rst=RST)

# Set variables
# replace with your own wallet ID (public key NOT private key)
wallet = "1PXLgcjt2SHtRTCLt1yyoqo69Gv6AocoJA"
balance_url = "https://blockchain.info/q/addressbalance/" + wallet

# Initialize library.
disp.begin()

# Get display width and height.
width = disp.width
height = disp.height

# Clear display.
disp.clear()
disp.display()

# Create image buffer.
# Make sure to create image with mode '1' for 1-bit color.
image = Image.new('1', (width, height))

# Load default font.
#font = ImageFont.load_default()

# Alternatively load a TTF font.  Make sure the .ttf font file is in the same directory as this python script!
# Some nice fonts to try: http://www.dafont.com/bitmap.php
font = ImageFont.truetype('BEBAS.ttf', 15)
line1 = 0
line2 = 16
# Create drawing object.
draw = ImageDraw.Draw(image)

# Animate text moving in sine wave.
#print('Press Ctrl-C to quit.')
while True:
    r = requests.get(balance_url)
    ticker = requests.get("https://blockchain.info/ticker").json()
    btc = int(r.text)/100000000
    price = ticker['GBP']['15m']
    wal_val = btc*price
    text1 = "Bal: " + str(btc) + " BTC."
    #print(text1)
    text2 = "Worth  £" + str('{:7.2f}'.format(wal_val))
    #print(text2)
    draw.rectangle((0,0,width,height), outline=0, fill=0)
    # Enumerate characters and draw them offset vertically based on a sine wave.
    x = 0
    # Line 1 on display
    for i, c in enumerate(text1):
        # Stop drawing if off the right side of screen.
        if x > width:
            break
        # Calculate width but skip drawing if off the left side of screen.
        if x < -10:
            char_width, char_height = draw.textsize(c, font=font)
            x += char_width
            continue
        # Calculate offset from sine wave.
        y = line1
        # Draw text. +math.floor(amplitude*math.sin(x/float(width)*2.0*math.pi))
        draw.text((x, y), c, font=font, fill=255)
        # Increment x position based on chacacter width.
        char_width, char_height = draw.textsize(c, font=font)
        x += char_width
    # Line 2 on Display
    x = 0
    for i, c in enumerate(text2):
        # Stop drawing if off the right side of screen.
        if x > width:
            break
        # Calculate width but skip drawing if off the left side of screen.
        if x < -10:
            char_width, char_height = draw.textsize(c, font=font)
            x += char_width
            continue
        # Calculate offset from sine wave.
        y = line2
        # Draw text. +math.floor(amplitude*math.sin(x/float(width)*2.0*math.pi))
        draw.text((x, y), c, font=font, fill=255)
        # Increment x position based on chacacter width.
        char_width, char_height = draw.textsize(c, font=font)
        x += char_width
    # Draw the image buffer.
    disp.image(image)
    disp.display()
    time.sleep(60)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
