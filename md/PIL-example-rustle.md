---
title: PIL example rustle (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'rustle'

Functions in program: 
* `def get_rustle_frame(im, rustle_size):`

Modules used in program: 
* `import sys`

## python rustle

Python PIL example: rustle

```python
#!/usr/bin/env python3

'''

Usage:

    ./rustle.py <input_file> <rustle_range> <frame_count> <output_file>

'''

import sys
from itertools import chain, repeat
from io import BytesIO
from shutil import copyfileobj
from random import randint
from subprocess import Popen, PIPE
from PIL import Image
from PIL.ImageOps import crop
from PIL.ImageChops import offset


def get_rustle_frame(im, rustle_size):
    return crop(
        offset(
            im,
            randint(-rustle_size, rustle_size),
            randint(-rustle_size, rustle_size)
        ),
        rustle_size
    )

if __name__ == '__main__':
    *filename, ext = sys.argv[1].split('.')
    filename = ''.join(filename)
    im = Image.open('.'.join((filename, ext)))
    #w, h = im.size
    #result_size = '{}x{}'.format(w - 2*int(sys.argv[2]), h - 2*int(sys.argv[2]))

    args = ['ffmpeg', '-framerate', '20', '-i', 'pipe:0', '-c:v', 'libx264', '-r', '30', '-pix_fmt', 'yuv420p', sys.argv[4]]
    #args = ['ffmpeg', '-f', 'rawvideo', '-framerate', '20', '-pixel_format', 'rgb24', '-video_size', result_size, '-i', 'pipe:0', '-c:v', 'libx264', '-r', '30', '-pix_fmt', 'yuv420p', sys.argv[4]]

    with Popen(args, stdin=PIPE) as ffmpeg:
        for i in range(int(sys.argv[3])):
            frame = get_rustle_frame(im, int(sys.argv[2]))
            #ffmpeg.stdin.write(im.tobytes())
            frame.save(ffmpeg.stdin, format='bmp')
    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
