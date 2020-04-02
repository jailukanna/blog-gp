---
title: PIL example Sound2image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Sound2image'


Modules used in program: 
* `import sys`
* `import math`
* `import numpy as np`

## python Sound2image

Python PIL example: Sound2image

```python
#Encode sound to image
#Requirement : Python 2.5, Numpy, audiolab, PIL
import numpy as np
from scikits.audiolab import Format, Sndfile
import math
from PIL import Image
import sys
 
if len(sys.argv)!=4:
    print('Syntax: input_file image_width output_file')
    sys.exit(1)
 
f=Sndfile(sys.argv[1], 'r')
data=f.read_frames(f.nframes)
 
#Generate image
IMGwidth=int(sys.argv[2])
IMGheight=math.ceil(f.nframes/IMGwidth)
IMG=Image.new('L', (IMGwidth, IMGheight), 127)
pixels=((data+1)/2)*255
x=0
y=0
for i in range(0, len(pixels)):
    if x&gt;=IMGwidth:
        x=0
        y=y+1
    if y&gt;=IMGheight:
        break
    IMG.putpixel((x, y), int(pixels[i]))
    x=x+1
 
IMG.save(sys.argv[3])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
