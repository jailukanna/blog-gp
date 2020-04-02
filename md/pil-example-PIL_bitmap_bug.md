---
title: pil example PIL bitmap bug (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'PIL bitmap bug'

Functions in program: 
* `def img_fromarray_grey(data):`
* `def img_fromarray_alt(data):`
* `def img_fromarray_ok(data):`
* `def img_fromarray_bad(data):`
* `def img_frombytes(data):`
* `def img_grey(data):`
* `def decode_pattern(s):`

Modules used in program: 
* `import numpy as np`
* `import tkinter as tk`

## python PIL bitmap bug

Python pil example: PIL bitmap bug

```python
#! /usr/bin/env python3

''' Demonstrate the PIL .fromarray bitmap bug

    PIL's Image.fromarray function has a bug with mode '1' images

    See
    https://stackoverflow.com/questions/2761645/error-converting-pil-bw-images-to-numpy-arrays

    Written by PM 2Ring 2017.06.26
'''

import tkinter as tk
import numpy as np
from PIL import Image, ImageTk

class Viewer:
    ''' View a list of PIL Images in separate windows '''
    def __init__(self, imglist):
        self.root = tk.Tk()
        tk.Label(self.root, text='Main Window').pack()
        for title, img in imglist:
            self.show(img, title)
        self.root.mainloop()

    def show(self, img, title):
        win = tk.Toplevel(self.root)
        win.title(title)
        win.photo = ImageTk.PhotoImage(img)
        tk.Label(win, image=win.photo).pack()
        tk.Label(win, text=title).pack()

# Some test patterns
gliders = '''\
.o....o.
..o..o..
ooo..ooo
........
ooo..ooo
..o..o..
.o....o.
'''

diamond = '''\
...o....
..o.o...
.o...o..
o.....o.
.o...o..
..o.o...
...o....
'''

def decode_pattern(s):
    ''' Convert a bitmap pattern from string format to a 2D list of ints '''
    rows = s.splitlines()
    width = max(map(len, rows))
    return [[int(c not in ' .') for c in row.ljust(width)] for row in rows]

# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

# Various ways to convert a 2D Numpy binary array to a bitmap image

# The standard work-around: first convert to greyscale 
def img_grey(data):
    return Image.fromarray(data * 255, mode='L').convert('1')

# Use .frombytes instead of .fromarray. 
# This is >2x faster than img_grey
def img_frombytes(data):
    size = data.shape[::-1]
    databytes = np.packbits(data, axis=1)
    return Image.frombytes(mode='1', size=size, data=databytes)

# The broken one
def img_fromarray_bad(data):
    databytes = np.packbits(data, axis=1)
    return Image.fromarray(databytes, mode='1')

# This works... if the scale's a multiple of 8
# Otherwise, there's vertical distortion due to lost pixels
def img_fromarray_ok(data):
    data = np.tile(data, (8, 8))
    data = data.repeat(scale, axis=1).repeat(scale // 8, axis=0)
    databytes = np.packbits(data, axis=1)
    return Image.fromarray(databytes, mode='1')

# Another attempt that works if the scale % 8 == 0. 
# This one gets the size correct when scale % 8 != 0
def img_fromarray_alt(data):
    data = np.tile(data, (8, 8))
    data = data[::8, ::1]
    data = np.packbits(data, axis=1)
    return Image.fromarray(data, mode='1')

# Also correct when scale % 8 == 0, and has the correct size otherwise.
# But it gets very wonky for odd scales.
def img_fromarray_grey(data):
    data *= 255
    data = np.tile(data, (8, 8))
    data = data[::8, ::8]
    return Image.fromarray(data, mode='1')

# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

pattern = diamond

# Convert the pattern to a 2D list...
bits = decode_pattern(pattern)
#print(*map(str, bits), sep=',\n')

# ... and a Numpy array
arr = np.array(bits, dtype=np.uint8)
#print(arr)

scale = 12

# Scale the image bits
big_arr = arr.repeat(scale, axis=1).repeat(scale, axis=0)

imglist = [
    ('grey', img_grey(big_arr)),
    ('frombytes', img_frombytes(big_arr)),
    ('fromarray_ok', img_fromarray_ok(arr)),
    ('fromarray_alt', img_fromarray_alt(big_arr)),
    ('fromarray_grey', img_fromarray_grey(big_arr)),
    ('fromarray_bad', img_fromarray_bad(big_arr)),
]

print('Sizes')
for title, img in imglist:
    print('{:15}: {}'.format(title, img.size))

Viewer(imglist)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
