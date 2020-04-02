---
title: pil example default8 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'default8'


Modules used in program: 
* `import os`
* `import argparse`

## python default8

Python pil example: default8

```python
#!/usr/bin/python3

import argparse
import os
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

# need fix
d8 = ('\u00c0\u00c1\u00c2\u00c8\u00ca\u00cb\u00cd\u00d3'
      '\u00d4\u00d5\u00da\u00df\u00e3\u00f5\u011f\u0130'
      '\u0131\u0152\u0153\u015e\u015f\u0174\u0175\u017e'
      '\u0207\u00a7\u00a9\u0000\u0000\u0000\u0000\u0000'
      '\u0020\u0021\u0022\u0023\u0024\u0025\u0026\u0027'
      '\u0028\u0029\u002a\u002b\u002c\u002d\u002e\u002f'
      '\u0030\u0031\u0032\u0033\u0034\u0035\u0036\u0037'
      '\u0038\u0039\u003a\u003b\u003c\u003d\u003e\u003f'
      '\u0040\u0041\u0042\u0043\u0044\u0045\u0046\u0047'
      '\u0048\u0049\u004a\u004b\u004c\u004d\u004e\u004f'
      '\u0050\u0051\u0052\u0053\u0054\u0055\u0056\u0057'
      '\u0058\u0059\u005a\u005b\u005c\u005d\u005e\u005f'
      '\u0060\u0061\u0062\u0063\u0064\u0065\u0066\u0067'
      '\u0068\u0069\u006a\u006b\u006c\u006d\u006e\u006f'
      '\u0070\u0071\u0072\u0073\u0074\u0075\u0076\u0077'
      '\u0078\u0079\u007a\u007b\u007c\u007d\u007e\u0000'
      '\u00c7\u00fc\u00e9\u00e2\u00e4\u00e0\u00e5\u00e7'
      '\u00ea\u00eb\u00e8\u00ef\u00ee\u00ec\u00c4\u00c5'
      '\u00c9\u00e6\u00c6\u00f4\u00f6\u00f2\u00fb\u00f9'
      '\u00ff\u00d6\u00dc\u00f8\u00a3\u00d8\u00d7\u0192'
      '\u00e1\u00ed\u00f3\u00fa\u00f1\u00d1\u00aa\u00ba'
      '\u00bf\u00ae\u00ac\u00bd\u00bc\u00a1\u00ab\u00bb'
      '\u2591\u2592\u2593\u2502\u2524\u2561\u2562\u2556'
      '\u2555\u2563\u2551\u2557\u255d\u255c\u255b\u2510'
      '\u2514\u2534\u252c\u251c\u2500\u253c\u255e\u255f'
      '\u255a\u2554\u2569\u2566\u2560\u2550\u256c\u2567'
      '\u2568\u2564\u2565\u2559\u2558\u2552\u2553\u256b'
      '\u256a\u2518\u250c\u2588\u2584\u258c\u2590\u2580'
      '\u03b1\u03b2\u0393\u03c0\u03a3\u03c3\u03bc\u03c4'
      '\u03a6\u0398\u03a9\u03b4\u221e\u2205\u2208\u2229'
      '\u2261\u00b1\u2265\u2264\u2320\u2321\u00f7\u2248'
      '\u00b0\u2219\u00b7\u221a\u207f\u00b2\u25a0\u0000')

parser = argparse.ArgumentParser(description='Generate default8.png')
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
args = parser.parse_args()
fntname = args.font
xoffset = args.xoffset
yoffset = args.yoffset
fntsize = args.size

img = Image.new('RGBA', (1024, 1024), (255, 255, 255, 0))
dr = ImageDraw.Draw(img)
# cannnot open font file, cause OSError
fnt = ImageFont.truetype(fntname, fntsize)

x = 0
y = 0

for ch in d8:
    if x == 16:
        x = 0
        y += 1
    xpos = x * 64 + xoffset
    ypos = y * 64 + yoffset
    dr.text((xpos, ypos), ch, font=fnt, fill=(255, 255, 255, 255))
    x += 1

fntname = os.path.basename(fntname)
fntdir = os.path.splitext(fntname)[0]
os.makedirs(fntdir, exist_ok=True)
os.chdir(fntdir)
img.save('default8.png')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com