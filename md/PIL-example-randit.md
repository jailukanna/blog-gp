---
title: PIL example randit (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'randit'

Functions in program: 
* `def randimg(size(100, 100), format='PNG'):`
* `def randstr(size=16):`

Modules used in program: 
* `import random`

## python randit

Python PIL example: randit

```python
import random
try:
    from io import StringIO
except ImportError:
    try:
        from cStringIO import StringIO
    except ImportError:
        from StringIO import StringIO

from PIL import Image


def randstr(size=16):
    """Generates random string
    
    Parameters:
        - size: integer, string size in byte
    
    returns: bytes, random string with `size` bytes
    
    """
    return (hex(random.randint(0, 2 ** (size * 8)))[2:]
             .rstrip('L').rjust(size * 2, '0')
             .decode('hex'))


def randimg(size(100, 100), format='PNG'):
    """Generates random image
    
    Parameters:
        - size: tuple(width, height), width and height of the image in pixel
        - format: string, any type that PIL accepted
    
    returns: bytes of the random image
    
    """
    im = Image.fromstring('RGBA', size,
                          randstr(size[0] * size[1] * 4))
    dstio = StringIO()
    im.save(dstio, format)
    return dstio.getvalue()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
