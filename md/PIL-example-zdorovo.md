---
title: PIL example zdorovo (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'zdorovo'

Functions in program: 
* `def memeh(s):`
* `def sarcasm():`

Modules used in program: 
* `import PIL`

## python zdorovo

Python PIL example: zdorovo

```python
# -*- coding: utf-8 -*-

import PIL
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
from contextlib import contextmanager
from subprocess import Popen

@contextmanager
def sarcasm():
  try:
    yield
  except Exception, e:
    memeh(e.__class__.__name__ + ": " + e.message)
    raise


def memeh(s):
  topString = s

  img = Image.open('zdorovo.jpg')
  imageSize = img.size

  fontSize = imageSize[1]/5
  font = ImageFont.truetype("Impact.ttf", fontSize)
  topTextSize = font.getsize(topString)
  while topTextSize[0] > imageSize[0]-20:
    fontSize = fontSize - 1
    font = ImageFont.truetype("Impact.ttf", fontSize)
    topTextSize = font.getsize(topString)

  topTextPositionX = (imageSize[0]/2) - (topTextSize[0]/2)
  topTextPositionY = 0
  topTextPosition = (topTextPositionX, topTextPositionY)

  draw = ImageDraw.Draw(img)

  outlineRange = fontSize/30
  for x in range(-outlineRange, outlineRange+1):
    for y in range(-outlineRange, outlineRange+1):
      draw.text((topTextPosition[0]+x, topTextPosition[1]+y), topString, (0,0,0), font=font)

  draw.text(topTextPosition, topString, (255,255,255), font=font)

  img.save("out.png")
  Popen(["feh", "out.png"])

with sarcasm():
  print( 1 / 0 )

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
