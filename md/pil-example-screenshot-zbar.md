---
title: pil example screenshot-zbar (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'screenshot-zbar'


Modules used in program: 
* `import StringIO`
* `import Image`
* `import zbar`
* `import sys`

## python screenshot-zbar

Python pil example: screenshot-zbar

```python
import sys
import zbar
import Image
import StringIO
from PySide import QtCore, QtGui
from PySide.QtCore import QBuffer

app = QtGui.QApplication(sys.argv)

buffer = QBuffer()
qimage = QtGui.QPixmap.grabWindow(QtGui.QApplication.desktop().winId()).save(buffer, "png")
png = buffer.data().data()
del(buffer)

# create a reader
scanner = zbar.ImageScanner()

# configure the reader
scanner.parse_config('enable')

# obtain image data
pil = Image.open(StringIO.StringIO(png)).convert('L')
del(png)
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

# clean up
del(image)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
