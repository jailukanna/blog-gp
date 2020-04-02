---
title: pil example imagekit-watermark (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'imagekit-watermark'


## python imagekit-watermark

Python pil example: imagekit-watermark

```python
from PIL import Image
from PIL import ImageFilter
from PIL import ImageOps
from PIL import ImageDraw
from PIL import ImageFont
from PIL import ImageColor
from django.conf import settings

class TextWatermark(object):
    def __init__(self, text):
        self.text = text
    def process(self, image):
        width = image.size[0]
        font = ImageFont.truetype(settings.WATERMARK_FONT, width/16)
        fillcolor = "white"
        shadowcolor = "black"
        x = 0
        y = 0
        draw = ImageDraw.Draw(image)
        draw.text((x-1, y), self.text, font=font, fill=shadowcolor)
        draw.text((x+1, y), self.text, font=font, fill=shadowcolor)
        draw.text((x, y-1), self.text, font=font, fill=shadowcolor)
        draw.text((x, y+1), self.text, font=font, fill=shadowcolor)
        draw.text((x, y),   self.text, font=font, fill=fillcolor)
        del draw
        return image


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
