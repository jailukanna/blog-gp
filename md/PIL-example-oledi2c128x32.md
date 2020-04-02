---
title: PIL example oledi2c128x32 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'oledi2c128x32'


Modules used in program: 
* `import Adafruit_SSD1306`
* `import Adafruit_GPIO.SPI as SPI`

## python oledi2c128x32

Python PIL example: oledi2c128x32

```python
import Adafruit_GPIO.SPI as SPI
import Adafruit_SSD1306

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

disp = Adafruit_SSD1306.SSD1306_128_32(rst=24)

disp.begin()
disp.clear()
disp.display()

image = Image.new('1', (disp.width, disp.height))

draw = ImageDraw.Draw(image)
draw.rectangle((0,0,disp.width-1,disp.height-1), outline=1, fill=0)

font = ImageFont.load_default()
draw.text((16, 8),'RASPBERRYTIPS.NL',  font=font, fill=255)

disp.image(image)
disp.display()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
