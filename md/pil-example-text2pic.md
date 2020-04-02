---
title: pil example text2pic (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'text2pic'

Functions in program: 
* `def cjkwrap(text, width, encoding="utf8"):`

Modules used in program: 
* `import re`
* `import sys`

## python text2pic

Python pil example: text2pic

```python
#!/usr/bin/env python2.7
# coding:utf-8
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw

import sys
import re

fontSize = 18
charWidth = 60
fontPath = '/System/Library/Fonts/STHeiti Light.ttc'
pixelWidth = charWidth * (fontSize/2+1)

rx=re.compile(u"([\u2e80-\uffff])", re.UNICODE)
 
def cjkwrap(text, width, encoding="utf8"):
	return reduce(lambda line, word, width=width: '%s%s%s' %
		(line, [' ','\n', ''][(len(line)-line.rfind('\n')-1
				+ len(word.split('\n',1)[0] ) >= width) or
				line[-1:] == '\0' and 2], word),
				rx.sub(r'\1\0 ', unicode(text,encoding)).split(' ')
		).replace('\0', '').encode(encoding)


lines = []

for l in sys.stdin:
	l = l.rstrip('\n')
	lines.extend(cjkwrap(l, charWidth).split('\n'))

pixelHeight = (fontSize+1) * len(lines)

font = ImageFont.truetype(fontPath,fontSize)
img=Image.new("RGBA", (pixelWidth+4,pixelHeight),(245,245,245))
draw = ImageDraw.Draw(img)
for n,line in enumerate(lines):
	draw.text((4, n*(fontSize+1)),line.decode('utf8'),(0,0,0),font=font)
draw = ImageDraw.Draw(img)
img.save("output.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
