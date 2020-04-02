---
title: pil example packItem (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'packItem'


Modules used in program: 
* `import os`
* `import xml.sax.handler`
* `import xml.sax`
* `import xml`
* `import PIL`

## python packItem

Python pil example: packItem

```python
import PIL
from PIL import Image
import xml
import xml.sax
import xml.sax.handler
import os

imname = ["TopLeft", "TopEdge", "TopRight", 
	"LeftEdge", "Middle", "RightEdge",
	"BottomLeft", "BottomEdge", "BottomRight",
]

ims = []
size = None
for i in xrange(0, 9):
	im = Image.open("ItemTooltip"+imname[i]+".png")
	ims.append(im)
	size = im.size


totalSize = [size[0]*3, size[1]*3]
c = 0
im = Image.new("RGBA", totalSize)
for i in ims:
	im.paste(i, ((c%3)*size[0], (c/3)*size[1]))
	c = c+1

im.save("ItemTooltip.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
