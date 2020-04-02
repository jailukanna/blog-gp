---
title: pil example oled system (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'oled system'

Functions in program: 
* `def read_system(pbar, width):`
* `def cpu_bars():`
* `def get_ip():`

Modules used in program: 
* `import socket`
* `import subprocess`
* `import Adafruit_SSD1306`
* `import Adafruit_GPIO.SPI as SPI`
* `import time`

## python oled system

Python pil example: oled system

```python
#!/usr/bin/python

import time
import Adafruit_GPIO.SPI as SPI
import Adafruit_SSD1306
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont
import subprocess
import socket
from gpiozero import CPUTemperature, LoadAverage, DiskUsage
from signal import pause

def get_ip():
    # https://stackoverflow.com/a/28950776/381200
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    try:
        # doesn't even have to be reachable
        s.connect(('10.255.255.255', 1))
        IP = s.getsockname()[0]
    except:
        IP = '127.0.0.1'
    finally:
        s.close()
    return IP

bars = ['\u2581', '\u2582', '\u2583', '\u2584', '\u2585', '\u2586', '\u2587', '\u2588', ]
#bars = ['_', 'o', 'O', '#']

def cpu_bars():
    la = LoadAverage(minutes=1)
    i = int(100 * la.load_average)
    return bars[int((len(bars)-1) * i/100)], i

def read_system(pbar, width):
    cpu = CPUTemperature(min_temp=50, max_temp=90)
    disp_temp = 'Temp: {:3.2f}C'.format(cpu.temperature)

    la = LoadAverage(min_load_average=0, max_load_average=2)
    bar, val = cpu_bars()
    pbar += bar
    pbar = pbar[1:width]
    disp_load = "CPU{:s}{:2.0f}%".format(pbar, val)

    disk = DiskUsage()
    disp_du = "Disk: {:3.2f}".format(disk.usage)

    disp_net = "IP: {}".format(get_ip())

    system_report = "{}".format(disp_temp)
    system_report = "{}\n{}\n{}\n{}\n".format(disp_temp, disp_load, disp_du, disp_net)
    return system_report, pbar

RST = 0
padding = 0

disp = Adafruit_SSD1306.SSD1306_128_64(rst=RST)
disp.begin()

width = disp.width
height = disp.height
top = padding
bottom = height - padding

#font = ImageFont.load_default()
font = ImageFont.truetype("FreeSans.ttf", 12)
x=0
while True:
    data = read_system()

    bw = Image.new('1', (width, height))
    draw = ImageDraw.Draw(bw)
    draw.text((0, top), data, font=font, fill=255)

    disp.clear()
    disp.image(bw)
    disp.display()
    time.sleep(1)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
