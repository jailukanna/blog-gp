---
title: pil example decode (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'decode'

Functions in program: 
* `def read_barcode(path):`

Modules used in program: 
* `import zbar`

## python decode

Python pil example: decode

```python
#-*- coding: utf-8 -*-
import zbar
from PIL import Image

def read_barcode(path):
    scanner = zbar.ImageScanner()
    scanner.parse_config('enable')
    pil = Image.open(image).convert('L')
    width, height = pil.size
    raw = pil.tostring()

    # wrap image data
    image = zbar.Image(width, height, 'Y800', raw)

    # scan the image for barcodes
    scanner.scan(image)

    # extract results
    for symbol in image:
        # do something useful with results
        print('decoded', symbol.type, 'symbol', '"%s"' % symbol.data)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
