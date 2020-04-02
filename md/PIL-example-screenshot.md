---
title: PIL example screenshot (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'screenshot'

Functions in program: 
* `def element(driver, el, padding=2):`
* `def grab(driver):`
* `def compare(ref, new):`
* `def _elementbox(el, padding=2):`

Modules used in program: 
* `import base64`

## python screenshot

Python PIL example: screenshot

```python
'''screenshot utilities for selenium

Let's you work with screenshots of specific elements as PIL images.

>>> el = driver.find_element_by_css_selecto('#logotype')
>>> img = screenshot.element(driver, el)
>>> ref = Image.open('reference.png')
>>> screenshot.compare(ref, img)
True
'''

from StringIO import StringIO
from PIL import Image, ImageChops
import base64


def _elementbox(el, padding=2):
    '''Get the dimensions of a box surrounding the element'''

    l = el.location
    s = el.size
    return (
        l['x'] - padding,
        l['y'] - padding,
        l['x'] + s['width'] + padding * 2,
        l['y'] + s['height'] + padding * 2
    )


def compare(ref, new):
    '''Compare two images, if they are equal return True'''

    if ref.size != new.size:
        return False

    diff = ImageChops.difference(ref, new)
    # If there's only one color in the diff it's a match
    return len(diff.getcolors()) == 1


def grab(driver):
    '''Grab a screenshot of the entire view as a PIL.Image'''

    data = driver.get_screenshot_as_base64()
    return Image.open(StringIO(base64.decodestring(data)))


def element(driver, el, padding=2):
    '''Get screenshot cropped to only include the given element'''

    box = _elementbox(el, padding)
    orig = grab(driver)
    return orig.crop(box)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
