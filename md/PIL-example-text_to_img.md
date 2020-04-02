---
title: PIL example text to img (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'text to img'


Modules used in program: 
* `import subprocess`
* `import io`
* `import sys`

## python text to img

Python PIL example: text to img

```python
#! /usr/bin/env python
# coding: utf-8
# coding=utf-8
# -*- coding: utf-8 -*-
# vim: fileencoding=utf-8
import sys
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
import io
import subprocess
buf = io.BytesIO()

fontFile = 'KouzanGyoushoOTF.otf'
font = ImageFont.truetype(fontFile, 64, encoding='utf-8')

text = '謹賀新年'
w, h = font.getsize(text)

img = Image.new('RGB', (w, h), (255,255,255))
draw = ImageDraw.Draw(img)
draw.text((0, 0), text, fill=(0,0,0), font=font)

img.save(buf, 'PNG')

# print(out)
p = subprocess.Popen('lpr', stdin=subprocess.PIPE)
p.communicate(buf.getvalue())

p.stdin.close()
buf.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
