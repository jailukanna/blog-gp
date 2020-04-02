---
title: pil example zbar test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'zbar test'


Modules used in program: 
* `import PIL.Image`
* `import zbar`
* `import sys`

## python zbar test

Python pil example: zbar test

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

### cf. http://99blues.dyndns.org/blog/2010/12/zbar/

import sys
import zbar
import PIL.Image

if len(sys.argv) < 2: exit(1)

# create a reader
scanner = zbar.ImageScanner()

# configure the reader
scanner.parse_config('enable')

# obtain image data
pil = PIL.Image.open(sys.argv[1]).convert('L')
(width, height) = pil.size
raw = pil.tostring()

# wrap image data
image = zbar.Image(width, height, 'Y800', raw)

# scan the image for barcodes
scanner.scan(image)

# extract results
for symbol in image:
    # do something useful with results
    print('decoded', symbol.type, 'symbol', '"%s"' % symbol.data)

# clean up
del(image)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
