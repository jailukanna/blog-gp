---
title: PIL example oledsystemdemo (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'oledsystemdemo'

Functions in program: 
* `def write_temp_mem():`
* `def write_load():`
* `def run_cmd(cmd):`

Modules used in program: 
* `import Adafruit_SSD1306`
* `import Adafruit_GPIO.SPI as SPI`
* `import subprocess`
* `import sys`
* `import time`

## python oledsystemdemo

Python PIL example: oledsystemdemo

```python
import time
import sys
import subprocess

import Adafruit_GPIO.SPI as SPI
import Adafruit_SSD1306

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

from time import sleep

# commandline instructies om de gegevens op te vragen

GET_LOADAVG_CMD = "grep 'cpu ' /proc/stat | awk '{usage=($2+$4)*100/($2+$4+$5)} END {print(usage }'")
GET_TEMP_CMD = "/opt/vc/bin/vcgencmd measure_temp"

def run_cmd(cmd):
    return subprocess.check_output(cmd, shell=True).decode('utf-8')

def write_load():
    load = float ( run_cmd(GET_LOADAVG_CMD)[:-1] )
    cpu = "CPU {:.2f}%  \n".format(load)
    draw.text((8, 7), cpu, font=font, fill=255)

def write_temp_mem():
    cputemp = "TEMP {} C".format( run_cmd(GET_TEMP_CMD)[5:9] )
    draw.text((8, 19), cputemp, font=font, fill=255)


disp = Adafruit_SSD1306.SSD1306_128_32(rst=24)

disp.begin()
disp.clear()
disp.display()

# Lege afbeelding voorbereiden

width = disp.width
height = disp.height
image = Image.new('1', (width, height))
draw = ImageDraw.Draw(image)

# Apple 2 font moet in dezelfde map staan als het script

font = ImageFont.truetype('Apple2.ttf', 8)

# loop voor het weergeven van de informatie

while True:
    draw.rectangle((0,0,width-1,height-1), outline=1, fill=0)
    write_load()
    write_temp_mem()
    disp.image(image)
    disp.display()
    sleep(3)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
