---
title: PIL example compress (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'compress'


Modules used in program: 
* `import sys`
* `import PIL`

## python compress

Python PIL example: compress

```python
# revalo

# Usage
# python compress.py input.jpg 6000 3000 <compression_level, example=40> output.jpg

import PIL
from PIL import Image
import sys

args = sys.argv[1:]

imageIn = args[0]
width = int(args[1])
height = int(args[2])
compress = int(args[3])
output = args[4]

img = Image.open(imageIn)
img = img.resize((width,height), PIL.Image.ANTIALIAS)
img.save(output,optimize=True,quality=compress)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
