---
title: PIL example phash0 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'phash0'

Functions in program: 
* `def alpha_to_color(image, color=(255, 255, 255)):`

Modules used in program: 
* `import shutil`
* `import os.path`
* `import argparse`
* `import os`

## python phash0

Python PIL example: phash0

```python
import os
import argparse
import os.path
import shutil

from PIL import Image
from glob import glob

# http://stackoverflow.com/questions/9166400/convert-rgba-png-to-rgb-with-pil
#def pure_pil_alpha_to_color_v2(image, color=(255, 255, 255)):
def alpha_to_color(image, color=(255, 255, 255)):
    """Alpha composite an RGBA Image with a specified color.

    Simpler, faster version than the solutions above.

    Source: http://stackoverflow.com/a/9459208/284318

    Keyword Arguments:
    image -- PIL RGBA Image object
    color -- Tuple r, g, b (default 255, 255, 255)

    """
    image.load()  # needed for split()
    background = Image.new('RGB', image.size, color)
    background.paste(image, mask=image.split()[3])  # 3 is the alpha channel
    return background


if __name__ == '__main__':

    parser = argparse.ArgumentParser(description=' resources.')
    parser.add_argument("-i", "--input", action="store", help="Input directory", required=True)
    parser.add_argument("-o", "--output", action="store", help="Output directory", required=True)

    args = vars(parser.parse_args())
    inpath = args["input"]
    outpath = args["output"]
    if not os.path.exists(outpath):
        os.makedirs(outpath)

    files = glob(inpath + os.sep + "*.png")
   
    for f in files:
        im = Image.open(f)
        w,h = im.size
        if (w*h < 100):
            print("Skipping file '{0}' due to small size ({},{}).".format(f,w,h))
            continue

        im = alpha_to_color(im)
        im = im.resize( (w*4, h*4), Image.NEAREST)
        f_new = os.path.basename(f)
        f_new = os.path.join(outpath, f_new)
        im.save(f_new)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
