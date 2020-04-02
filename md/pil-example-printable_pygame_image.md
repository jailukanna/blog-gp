---
title: pil example printable pygame image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'printable pygame image'


Modules used in program: 
* `import pygame`
* `import base64`
* `import io`

## python printable pygame image

Python pil example: printable pygame image

```python
import io
import base64

import pygame
from PIL import Image

pygame.init()

class PrintablePygameImage:
    BASE64_PNG_PREFIX = bytes("data:image/png;base64,", encoding='ascii')
    
    def __init__(self, size):
        self._s = pygame.Surface(size, pygame.SRCALPHA)
    
    def rotate(self, angle):
        self._s = pygame.transform.rotate(self._s, angle)
    
    def circle(self, color, position, radius):
        pygame.draw.circle(self._s, color, position, radius)
    
    def __repr__(self):
        surf_as_str = pygame.image.tostring(self._s, 'RGBA')
        surf_as_pil = Image.frombytes('RGBA', self._s.get_size(), surf_as_str)
        surf_as_bytes = io.BytesIO()
        surf_as_pil.save(surf_as_bytes, format="png")
        base64_png = base64.b64encode(surf_as_bytes.getvalue())
        base64_png = self.BASE64_PNG_PREFIX + base64_png
        return base64_png.decode('utf-8')


s = PrintablePygameImage((100, 100))
s.circle((255, 0, 0), (10, 10), 10)
s.circle((0, 255, 0), (50, 50), 10)
s.circle((0, 0, 255), (90, 90), 10)
print(s)
s.rotate(90)
print(s)
s.rotate(-45)
print(s)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
