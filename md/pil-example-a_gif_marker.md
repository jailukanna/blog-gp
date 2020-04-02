---
title: pil example a gif marker (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'a gif marker'


Modules used in program: 
* `import os`

## python a gif marker

Python pil example: a gif marker

```python
try:
    from images2gif_master.images2gif import writeGif
except:
    from images2gif import writeGif

from PIL import Image
import os

folder = r'C:\Users\Public\Pictures\Sample Pictures'

file_names = sorted((folder + '\\' + fn for fn in os.listdir(folder) if fn.endswith('.jpg')))
# ['animationframa.png', 'animationframb.png', ...] "
print('file_names', file_names)

images = [Image.open(fn) for fn in file_names]

size = (150, 150)
for im in images:
    im.thumbnail(size, Image.ANTIALIAS)

print(writeGif.__doc__)

filename = "my_gif.gif"
writeGif(filename, images, duration=0.2)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
