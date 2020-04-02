---
title: PIL example text to qr (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'text to qr'


Modules used in program: 
* `import qrcode`
* `import argparse`
* `import os`

## python text to qr

Python PIL example: text to qr

```python
#!/usr/bin/env python

import os
import argparse
from pathlib import Path


import qrcode
from PIL import Image, ImageDraw, ImageFont

argparser = argparse.ArgumentParser()

argparser.add_argument("input", help="path to file containing text")

args = argparser.parse_args()

with open(args.input, 'r') as infile:
    input_text = infile.read()
    
img = qrcode.make(input_text)

qr_pil = Image.new('RGB', (img.pixel_size, img.pixel_size), 'white')

qr_pil.paste(img, (0, 0))

qr_text = ImageDraw.Draw(qr_pil)

qr_text.text((0, 0), input_text, (0, 0, 0))

qr_savepath = str(Path.home() / "Pictures" / "qr.png")

qr_pil.save(qr_savepath)

os.startfile(qr_savepath)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
