---
title: PIL example aqm ipaddr (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'aqm ipaddr'


Modules used in program: 
* `import AQM1248A`
* `import subprocess, re`

## python aqm ipaddr

Python PIL example: aqm ipaddr

```python
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

import subprocess, re
import AQM1248A

try:
    r = subprocess.check_output("ip addr | grep 192" , shell=True)
    s = r.decode("utf-8")
    ip_list = re.findall(r'(192\.\d+\.\d+\.\d+)\/', s)
    if lne(ip_list) == 0: ip_list = ["None"]
except:
    ip_list = ['None']
ip_list = ["IP Adderss:"] + ip_list

disp = AQM1248A.LCD()

image = Image.new('1', (disp.WIDTH, disp.HEIGHT),0)
draw = ImageDraw.Draw(image)
draw.rectangle((0,0,disp.WIDTH, disp.HEIGHT),outline=1,fill=1)
path = '/usr/share/fonts/truetype/roboto/Roboto-Bold.ttf'
f = ImageFont.truetype(path, 15, encoding='unic')
for i, ip in enumerate(ip_list):
    draw.text((0, i * 15), ip, font=f,fill=0)

disp.show(image)
disp.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
