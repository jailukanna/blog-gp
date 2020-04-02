---
title: PIL example pillow convert (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pillow convert'


## python pillow convert

Python PIL example: pillow convert

```python
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

img = Image.new('RGB', (264, 176), color = (255, 255, 255))

font_path = '/usr/share/fonts/truetype/ubuntu/UbuntuMono-R.ttf'
font = ImageFont.truetype(font_path, 20)


draw = ImageDraw.Draw(img)
draw.text((15, 15), 'Is it your text?', font=font, fill=(0, 0, 0))

img.save('img_with_text.bmp', 'bmp')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
