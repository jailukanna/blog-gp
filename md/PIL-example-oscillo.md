---
title: PIL example oscillo (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'oscillo'


Modules used in program: 
* `import numpy`
* `import struct`
* `import subprocess`
* `import argparse`

## python oscillo

Python PIL example: oscillo

```python
#! /usr/bin/env python2
"""oscillo.py: Port of windytan's oscillo.pl to python.

Requires Sox (for resampling), PIL/Pillow, and Numpy
Original script: https://gist.github.com/windytan/5276653
"""

import argparse
import subprocess
import struct
import numpy
from PIL import Image


parser = argparse.ArgumentParser()
parser.add_argument('-f', type=str, help="PCM File", required=True)
parser.add_argument('-x', help="Samples per pixel x", type=float,
                    default=1200)
parser.add_argument('-y', help="Quant levels per pixel y", type=int,
                    default=100)
parser.add_argument('-t', type=int, help="Oversampling target rate",
                    default=1099961)
parser.add_argument('-G', type=float, help="Wave preamplification, dB",
                    default=0)
parser.add_argument('-g', type=float, help="Brightness added by one sample",
                    default=6)
parser.add_argument('-w', type=int, help="Image width",
                    default=1000)
parser.add_argument('-s', type=float, help="Skip amount of seconds",
                    default=0.05)

args = parser.parse_args()


# Generate turquoise gradient of RGB tuples.
# TODO: A 2D Numpy array would be much more suitable, but wouldn't match PIL
gradient = []
for i in range(0, 128):
    gradient.append((int((i / 2.0) + 0.5), int(i * 1.5), int(i * 1.5)))

for i in range(128, 256):
    gradient.append((
                    int(64  + (i - 128) * 1.5),
                    int(192 + (i - 128) / 2),
                    int(192 + (i - 128) / 2)))


# Open SOX and resample
log = open("sox.log", "w")
S = subprocess.check_output(["sox", args.f,
                            "-r %d" % args.t,
                             "-b 16",   # 16 bits
                             "-c 2",    # 2 channel
                             "-t.raw",  # Output without header
                             "-esigned-integer",
                             "-",       # Output to stdout
                             #'trim %s gain %f' % (args.s, args.G),
                             ], stderr=log)

# Unpack to unsigned short (int16), little endian
samples = numpy.frombuffer(S,dtype='int16')

# Remove right channel, not sure why we don't just downmix to mono instead
samples = samples[::2]

# Generate empty pixels array
pix = numpy.zeros((args.w+2,int(65536/args.y)))

# Iterate over each one, and change pixel to suit
for n, a in enumerate(samples):
    # Invert
    a = -a

    # Get pixel position
    x = n / args.x
    y = (a + 32768) / args.y

    # Use bilinear interpolation to set 4 pixel area
    xdec = x - int(x)
    ydec = y - int(y)

    pix[x][y]       += (1-xdec) * (1-ydec)
    pix[x+1][y]     += (xdec)   * (1-ydec)
    pix[x][y+1]     += (1-xdec) * (ydec)
    pix[x+1][y+1]   += (xdec)   * (ydec)

    # Exit loop if we have hit the end of the screen
    if (n/float(args.x) > args.w):
        break


# Create an image, black background
img = Image.new('RGB', size=(args.w, int(65536/args.y)))

# For X and Y
for y in range(0, int(65536/args.y)):
    for x in range(0, args.w):
        p = pix[x][y] * args.g # Apply gain
        p = min(p, 255) # Limit to 255
        img.putpixel((x,y), gradient[int(p)])

# Write to file
img.save("osc2.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
