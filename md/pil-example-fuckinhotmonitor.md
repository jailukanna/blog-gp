---
title: pil example fuckinhotmonitor (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'fuckinhotmonitor'


Modules used in program: 
* `import Adafruit_GPIO.SPI as SPI`
* `import Adafruit_Nokia_LCD as LCD`
* `import math`
* `import datetime`
* `import time`

## python fuckinhotmonitor

Python pil example: fuckinhotmonitor

```python
#!/usr/bin/env python

import time
import datetime
import math
import Adafruit_Nokia_LCD as LCD
import Adafruit_GPIO.SPI as SPI
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont
from zabbix.api import ZabbixAPI

# Path
path="/home/pi/fuckinhotmon"

# Zabbix Parameters
zbxurl=""
zbxuser=""
zbxpass=""
zbxitemid=

# threshold
threshold = 30

# Pictures
pic_normal = path+"/shachikuchan.png"
pic_fuckinhot = path+"/hot.png"

# RPi software SPI:
SCLK = 17
DIN = 18
DC = 27
RST = 23
CS = 22
disp = LCD.PCD8544(DC, RST, SCLK, DIN, CS)
disp.begin(contrast=60)
disp.clear()

fuckinhot = Image.new('1', (LCD.LCDWIDTH, LCD.LCDHEIGHT))
draw = ImageDraw.Draw(fuckinhot)

# http://www.dafont.com/minecraftia.font
font = ImageFont.truetype(path+'/minecraftia.ttf', 8)

z = ZabbixAPI(url=zbxurl, user=zbxuser, password=zbxpass)

# loop
while True:
	t = z.item.get(itemids=(zbxitemid))
	temp = round(float(t[0]["lastvalue"]), 2)
	if temp >= threshold:
		fuckinhot = Image.open(pic_hot).convert('1')
	else:
		fuckinhot = Image.open(pic_normal).convert('1')
	draw = ImageDraw.Draw(fuckinhot)
	draw.rectangle((48,0,84,8), outline=255, fill=255)
	draw.text((48,0), '%05s' % (temp) + '  C', font=font)

	d = datetime.datetime.today()
	draw.rectangle((0,0,24,8), outline=255, fill=255)
	draw.text(( 0,0), '%02d:%02d' % (d.hour, d.minute), font=font)

	disp.image(fuckinhot)
	disp.display()
	time.sleep(10.0)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
