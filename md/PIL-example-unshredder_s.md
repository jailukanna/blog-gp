---
title: PIL example unshredder s (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'unshredder s'

Functions in program: 
* `def log2(n):`
* `def hist_dist(ls, rs):`
* `def line_dist(ls, rs):`
* `def hist(vs, size=64):`
* `def hists(rgbs):`
* `def hsv(rgb):`
* `def guess(lhists):`
* `def ordering(lhists, shred):`
* `def unshred(image):`
* `def main():`

Modules used in program: 
* `import PIL.Image`
* `import itertools`
* `import fractions`
* `import colorsys`
* `import argparse`

## python unshredder s

Python PIL example: unshredder s

```python
# instagram engineering challenge: unshredder (short version)
# http://instagram-engineering.tumblr.com/post/12651721845/instagram-engineering-challenge-the-unshredder
# checked with Python 2.7.2 and PIL 1.1.7 on Ubuntu Oneiric

import argparse
import colorsys
import fractions
import itertools
import PIL.Image


def main():
    parser = argparse.ArgumentParser(description="Image Unshredder")
    parser.add_argument("-o", "--out-file", default="unshredded.png",
                        help="output image file name")
    parser.add_argument("imagefile", help="input shredded image file name")
    args = parser.parse_args()
    image = PIL.Image.open(args.imagefile)
    out = unshred(image)
    out.save(args.out_file, "PNG")
    pass

def unshred(image):
    lhists = [hists(image.getpixel((x, y)) for y in range(image.size[1]))
              for x in range(image.size[0])]
    shred = guess(lhists)
    order = ordering(lhists, shred)
    out = PIL.Image.new(image.mode, image.size)
    for i, n in enumerate(order):
        crop = image.crop((shred * n, 0, shred * (n + 1), image.size[1]))
        out.paste(crop, (shred * i, 0))
        pass
    return out

def ordering(lhists, shred):
    parts = (([i], lhists[shred * i], lhists[shred * (i + 1) - 1])
             for i in range(len(lhists) // shred))
    dists = ((line_dist(a[2], b[1]), a, b) 
             for a, b in itertools.permutations(parts, 2))
    queue = sorted(dists, key=lambda e: e[0])
    while True:
        closeness, l, r = queue[0]
        m = (l[0] + r[0], l[1], r[2])
        queue = [(d, m if lp == r else lp, m if rp == l else rp) 
                 for d, lp, rp in queue
                 if lp != l and rp != r and not (lp == r and rp == l)]
        if len(queue) == 0: return m[0]
        pass
    raise AssertionError("unreached")

def guess(lhists):
    minshred = max(len(lhists) // 100, 2)
    unsorted_dists = ((line_dist(lhists[rx - 1], lhists[rx]), rx) 
                      for rx in range(1, len(lhists)))
    dists = sorted(unsorted_dists, key=lambda t:t[0], reverse=True)
    shred = dists[0][1]
    for d in dists[1:]:
        val = fractions.gcd(shred, d[1])
        if val < minshred: break
        shred = val
        pass
    return shred

def hsv(rgb):
    return colorsys.rgb_to_hsv(rgb[0] / 255.0, rgb[1] / 255.0, rgb[2] / 255.0)

def hists(rgbs):
    return [hist(ps) for ps in zip(*(hsv(rgb) for rgb in rgbs))]

def hist(vs, size=64):
    r, m = [0] * size, size - 1
    for v in vs:
        r[int(v * m)] += 1
        pass
    return r

def line_dist(ls, rs):
    return sum(hist_dist(l, r) for l, r in zip(ls, rs))

def hist_dist(ls, rs):
    return sum((log2(l) - log2(r)) ** 2 for l, r in zip(ls, rs))

def log2(n):
    r, p2 = 0, 1
    while p2 < n:
        r, p2 = r + 1, p2 * 2
        pass
    return r

if __name__ == "__main__": main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
