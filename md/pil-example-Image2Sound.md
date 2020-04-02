---
title: pil example Image2Sound (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Image2Sound'


Modules used in program: 
* `import sys`
* `import math`
* `import numpy as np`

## python Image2Sound

Python pil example: Image2Sound

```python
#Encode image to sound
#Requirement : Python 2.5, Numpy, audiolab, PIL
 
import numpy as np
from scikits.audiolab import Format, Sndfile
import math
from PIL import Image
import sys
 
if len(sys.argv)!=2:
    print('Syntax: input_file output_file')
    sys.exit(1)
 
IMG=Image.open(sys.argv[1])
IMGwidth=IMG.size[0]
IMGheight=IMG.size[1]
nframes=IMGheight*IMGwidth
data=np.zeros(nframes)
i=0
for y in range(0, IMGheight):
    for x in range(0, IMGwidth):
        data[i]=IMG.getpixel((x, y))
        i=i+1
 
sndData=(data/255)*2-1
f=Sndfile(sys.argv[2], 'w', format=Format(), channels=1, samplerate=44100) #Samplerate may change but it is usually 44100 Hz
f.write_frames(sndData)
f.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
