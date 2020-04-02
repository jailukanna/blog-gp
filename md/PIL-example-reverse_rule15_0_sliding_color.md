---
title: PIL example reverse rule15 0 sliding color (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'reverse rule15 0 sliding color'

Functions in program: 
* `def get_image(m):`
* `def digital_reverse(n, length, base):`

Modules used in program: 
* `import numpy as np`

## python reverse rule15 0 sliding color

Python PIL example: reverse rule15 0 sliding color

```python
import numpy as np
from PIL import Image

np.set_printoptions(linewidth = 180, edgeitems=10, suppress = True)

def digital_reverse(n, length, base):
  r = 0
  for _ in range(length):
    r = base*r + n % base
    n /= base
  return r

assert digital_reverse(123, 3, 10) == 321
assert digital_reverse(54321, 5, 10) == 12345
assert digital_reverse(0x123, 3, 16) == 0x321
assert digital_reverse(0x54321, 5, 16) == 0x12345

color = [[lambda g: (g, 0, 0), lambda g: (0, g, 0)], [lambda g: (0, 0, g), lambda g: (g, g, 0)]]
def get_image(m):
    image = Image.new("RGB", (m.shape[1], m.shape[0]), 0)
    pix = image.load()
    for y in xrange(m.shape[0]):
        for x in xrange(m.shape[1]):
             #pix[x, y] = 255*m[y, x]/(modulo-1)
             pix[x, y] = color[x&1][y&1](m[y, x])

    return image
    

if __name__ == '__main__':
    import pylab as P

    # Image size
    base = 2
    power = 6

    size = base**power

    print("base", base)
    print("power", power)
    print("size", size)

    # known state after applying rule 15
    reversible_state = np.zeros((size, size), np.int8)
    for i in xrange(size):
        ri = digital_reverse(i, power, base)
        reversible_state[i, ri] = 1
    #reversible_state = np.ones((size, size), np.int8)

    # partially known state
    state = np.zeros((size, size), np.int16)

    # fill initial (known) values
    state[:, 0] = np.arange(size)
    state[0, :] = np.arange(size)

    # find rest values of the state
    for i0 in xrange(size-1):
        i1 = i0 + 1
        for j0 in xrange(size-1):
            j1 = j0 + 1
            # reverse rule 15-0
            state[i1, j1] = -state[i0, j0] - state[i0, j1] - state[i1, j0] + reversible_state[i0, j0]

    mn = np.min(state)
    mx = np.max(state)

    print("min cell", mn)
    print("max cell", mx)

    pic = np.zeros((size, size), np.int16)
    images = [None] * (mx-mn+1)
    for value in range(mn, mx+1):
      print("slide", value-mn, 'of', mx-mn)
      pic -= 20 # hiding tail
      pic[pic<0] = 0
      pic[state == value] = 255
       
      images[value-mn] = get_image(pic)

    from images2gif import writeGif
    writeGif("reverse_rule15_0_sliding_color.gif", images, duration=0.05, dither=False)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
