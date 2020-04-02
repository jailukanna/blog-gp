---
title: PIL example display (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'display'


Modules used in program: 
* `import Adafruit_SSD1306`
* `import Adafruit_GPIO.SPI as SPI`
* `import sys`

## python display

Python PIL example: display

```python
#!/usr/bin/python3
import sys
import Adafruit_GPIO.SPI as SPI
import Adafruit_SSD1306

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

disp = Adafruit_SSD1306.SSD1306_128_32(rst=None)
disp.begin()
#disp.clear()

if len(sys.argv)>1 :
    lines = [ ' '.join(sys.argv[1:]) ]
elif not sys.stdin.isatty() :
    lines = sys.stdin.readlines()
    if len(lines)>3 : lines = lines[:3]
else:
    lines = None

if lines:
    width = disp.width
    height = disp.height
    image = Image.new('1', (width, height))
    draw = ImageDraw.Draw(image)
    font = ImageFont.load_default()
    for y, line in enumerate(lines):
        draw.text(
            (0, y*8+2),
            line,
            font=font, fill=255
        )
    disp.image(image)

disp.display()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
