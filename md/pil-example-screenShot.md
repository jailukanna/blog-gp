---
title: pil example screenShot (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'screenShot'


Modules used in program: 
* `import gtk`
* `import ImageGrab`
* `import time`

## python screenShot

Python pil example: screenShot

```python
# -*- coding:utf-8 -*-

import time
import ImageGrab
import gtk


width = 1000
height = 1000
root_window = gtk.gdk.get_default_root_window()

gtkTime = 0
pilTime = 0

for i in range(10):
    #GTKの計測
    start = time.time()
    pixbuf = gtk.gdk.Pixbuf(gtk.gdk.COLORSPACE_RGB, False, 8, width, height)   #描写領域の確保
    pixbuf.get_from_drawable(root_window, gtk.gdk.colormap_get_system(), 0, 0, 0, 0, width, height) #スクリーンキャプチャ
    gtkTime += time.time() - start

    #PILの計測
    start = time.time()
    img = ImageGrab.grab((0, 0, width, height))   #スクリーンキャプチャ
    pilTime += time.time() - start

print(u'gtk', round(gtkTime / 10, 2))
print(u'PIL', round(pilTime / 10, 2))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
