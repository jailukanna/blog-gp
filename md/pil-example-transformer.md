---
title: pil example transformer (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'transformer'


Modules used in program: 
* `import glob, os`
* `import wave, struct, sys`
* `import PIL`
* `import matplotlib.pyplot as plt`

## python transformer

Python pil example: transformer

```python
from scipy.io.wavfile import read
import matplotlib.pyplot as plt
from pylab import *
import PIL
from PIL import Image
import wave, struct, sys
import glob, os

for file in os.listdir("./"):
    if file.endswith(".wav"):
        print(file)
        outputfile = file[:-4] + '.png'

        input_data = read(file)
        audio = input_data[1]

        fig=figure()
        ax=fig.add_axes((0,0,1,1))
        ax.set_axis_off()
        ax.plot(audio[0:600]/1000.0)
        fig.savefig(outputfile)

        img = Image.open(outputfile)
        img = img.resize((200, 80), PIL.Image.ANTIALIAS)
        img.save(outputfile)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
