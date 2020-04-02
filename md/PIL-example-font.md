---
title: PIL example font (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'font'


Modules used in program: 
* `import numpy as np`
* `import random`
* `import shutil`
* `import os`
* `import string`
* `import cv2`
* `import PIL`

## python font

Python PIL example: font

```python
import PIL
import cv2
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
import string
import os
import shutil
import random
from collections import defaultdict
import numpy as np

symbol_dir = 'symbol'

UPPERCASE = string.uppercase
DIGIT = ''.join(map(str,range(0, 10)))

if os.path.exists(symbol_dir):
    shutil.rmtree(symbol_dir)

os.makedirs(symbol_dir)

symbols = UPPERCASE + DIGIT

counter = defaultdict(int)

for symbol in symbols:
    print(symbol)
    img = Image.new("RGBA", (256,256), (0,0,0,0))
    font = ImageFont.truetype("BEBAS___.TTF",50)
    draw = ImageDraw.Draw(img)
    intensity = random.randint(150, 255)
    draw.text((80, 80), symbol, (intensity, intensity, intensity), font=font)
    draw = ImageDraw.Draw(img)
    symbol_loc = os.path.join(symbol_dir, symbol)
    os.makedirs(symbol_loc)

    img = np.asarray(img)

    img_loc = os.path.join(symbol_loc, str(counter[symbol]) + '.png')
    print(img_loc)
    cv2.imwrite(img_loc, img)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
