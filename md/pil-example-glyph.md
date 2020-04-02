---
title: pil example glyph (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'glyph'

Functions in program: 
* `def mkglyph(f, fnt, xof, yof):`

Modules used in program: 
* `import os`
* `import argparse`

## python glyph

Python pil example: glyph

```python
#!/usr/bin/python3

import argparse
import os
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont


def mkglyph(f, fnt, xof, yof):
    img = Image.new('RGBA', (1024, 1024), (255, 255, 255, 0))
    dr = ImageDraw.Draw(img)
    x = 0
    y = 0
    upper = f >> 8
    while 1:
        # x, y range 0-15, 16 to 0
        if x == 16:
            x = 0
            y += 1
            if y == 16:
                break
        # escape double height characters
        if f == 0x3031:
            x += 5
            f += 5
            continue
        # draw glyph file
        xpos = x * 64 + xof
        ypos = y * 64 + yof
        dr.text((xpos, ypos), chr(f), font=fnt, fill=(255, 255, 255, 255))
        # for next loop
        x += 1
        f += 1
    filename = 'glyph_' + '{:02X}'.format(upper) + '.png'
    img.save(filename)
    return f


parser = argparse.ArgumentParser(description='Generate glyph_XX.png')
parser.add_argument('font',
                    help='A filename of TrueType font')
parser.add_argument('-x', '--xoffset',
                    type=int,
                    default=0,
                    help='X offset, in pixels')
parser.add_argument('-y', '--yoffset',
                    type=int,
                    default=0,
                    help='Y offset, in pixels')
parser.add_argument('-s', '--size',
                    type=int,
                    default=64,
                    help='Font size, in points')
parser.add_argument('-a', '--asciionly',
                    action='store_true',
                    help='Generate ascii glyph only')
args = parser.parse_args()
fntname = args.font
xoffset = args.xoffset
yoffset = args.yoffset
fntsize = args.size

# cannnot open font file, cause OSError
fnt = ImageFont.truetype(fntname, fntsize)

fntname = os.path.basename(fntname)
fntdir = os.path.splitext(fntname)[0]
os.makedirs(fntdir, exist_ok=True)
os.chdir(fntdir)
unicodeint = 0
while unicodeint <= 0xffff:
    unicodeint = mkglyph(unicodeint, fnt, xoffset, yoffset)
    if args.asciionly:
        break
    # skip unicode private use area
    if unicodeint == 0xe000:
        unicodeint = 0xf900


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
