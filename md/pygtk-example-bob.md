---
title: pygtk example bob (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'bob'


Modules used in program: 
* `import pygtk`
* `import gobject`
* `import glib`
* `import pangocairo`
* `import pango`
* `import cairocffi as cairo`

## python bob

Python pygtk example: bob

```python
import cairocffi as cairo
import pango
import pangocairo
import glib
import gobject
import pygtk

WIDTH, HEIGHT = 256, 256

surface = cairo.ImageSurface(cairo.FORMAT_ARGB32, WIDTH, HEIGHT)
context = cairo.Context(surface)

context.select_font_face('Raleway-SemiBold', slant=cairo.FONT_SLANT_NORMAL, weight=cairo.FONT_WEIGHT_NORMAL)
context.set_font_size(40)
text = 'Hello World'


context.move_to(0, 100)
context.show_text(text)

surface.write_to_png("example.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
