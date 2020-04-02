---
title: pil example rangen (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'rangen'


Modules used in program: 
* `import subprocess`
* `import string`
* `import random`

## python rangen

Python pil example: rangen

```python
import random
import string
import subprocess
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw 
 
codes = []
 
for i in range(1, 251):
 
    char_pool = string.ascii_uppercase + string.digits
 
    # Generate a random code
    r = ''.join([random.choice(char_pool) for n in xrange(5)])
 
    # Check generated code against pre-existing codes.
    if r not in codes:
        codes.append(r)

        # Open template image and draw on it
        img = Image.open("template.png")
        draw = ImageDraw.Draw(img)  
        font = ImageFont.truetype("Inconsolata.otf", 72)
        draw.text((430, 527), str(i), (0,0,0), font=font)
        draw.text((430, 706), str(r), (0,0,0), font=font)

        # save 
        img.save('./cards/%02d%s.png' % (i, r))
 
        print("Created card for #%s, Code: %s" % (i, r))

print("Merging all cards into a single PDF...")

subprocess.call('convert ./cards/* output.pdf', shell=True)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
