---
title: pil example source generators (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'source generators'

Functions in program: 
* `def wand_image(source, **options):`

## python source generators

Python pil example: source generators

```python
try:
    from cStringIO import StringIO
except ImportError:
    from StringIO import StringIO

from PIL import Image as PILImage
from wand.image import Image

# Source Generator for the easy-thumbnails Django application
# https://github.com/SmileyChris/easy-thumbnails
#
# This generator fills in some of the gaps left by using only PIL.
# It uses Wand ( https://github.com/dahlia/wand ), which is an
# ImageMagick binding for Python. Result: More file formats!

def wand_image(source, **options):
    if not source:
        return

    try:
        # Read source file, output PNG blob and finally off to PIL for return
        raw = StringIO(Image(filename=str(source.file)).make_blob('png'))
        raw.seek(0)
        image = PILImage.open(raw)
    except Exception:
        return

    return image


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
