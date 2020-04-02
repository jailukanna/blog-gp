---
title: PIL example unshredder (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'unshredder'

Functions in program: 
* `def main():`

Modules used in program: 
* `import PIL.Image`
* `import itertools`
* `import colorsys`
* `import argparse`

## python unshredder

Python PIL example: unshredder

```python
# instagram engineering challenge: unshredder 
# http://instagram-engineering.tumblr.com/post/12651721845/instagram-engineering-challenge-the-unshredder
# checked with Python 2.7.2 and PIL 1.1.7 on Ubuntu Oneiric

import argparse
import colorsys
import itertools
import PIL.Image


def main():
    methods = [n for n in dir(Closeness) if n.startswith("by_")]
    parser = argparse.ArgumentParser(
        description="Image Unshredder",
        formatter_class=argparse.ArgumentDefaultsHelpFormatter)
    parser.add_argument("-o", "--out-file", default="unshredded.png",
                        help="output image file name")
    parser.add_argument("-f", "--out-format", default="PNG",
                        help="output image format")
    parser.add_argument("-s", "--shred-width", type=int, default=32,
                        help="width of each shredded of input image")
    parser.add_argument("-g", "--guess-width", action="store_true", 
                        default=False,
                        help="automatically guess width of each shred: " + 
                        "SHRED_WIDTH will be ignored")
    parser.add_argument("-m", "--closeness-method", default="by_hsv_hist",
                        help="method for evaluate closeness: select one of " + 
                        str(methods))
    parser.add_argument("imagefile", help="input shredded image file name")

    args = parser.parse_args()
    evaluator = getattr(Closeness, args.closeness_method)
    in_image = PIL.Image.open(args.imagefile)
    if args.guess_width:
        shred_width = ShredSize(in_image, evaluator).guess()
        pass
    else:
        shred_width = args.shred_width
        pass
    assert shred_width > 1
    out_image = Unshredder(in_image, shred_width, evaluator).run()
    out_image.save(args.out_file, args.out_format)
    pass


class Part(object):
    "fragmented part of image"
    def __init__(self, crops, crop_width, lefts=None, rights=None):
        assert len(crops) > 0
        assert all(crop.size[0] == crop_width for crop in crops)
        assert all(crop.size[1] == crops[0].size[1] for crop in crops)
        self.crops = crops
        self.crop_width = crop_width
        # cache side lines
        self.lefts = lefts or [crops[0].getpixel((0, y)) 
                               for y in range(crops[0].size[1])]
        self.rights = rights or [crops[-1].getpixel((crop_width - 1, y)) 
                                 for y in range(crops[-1].size[1])]
        pass

    def join(self, right):
        assert self.crop_width == right.crop_width
        return Part(self.crops + right.crops, self.crop_width,
                    self.lefts, right.rights)

    def to_image(self):
        width = sum(crop.size[0] for crop in self.crops)
        height = self.crops[0].size[1]
        image = PIL.Image.new(self.crops[0].mode, (width, height))
        for i in range(len(self.crops)):
            image.paste(self.crops[i], (self.crop_width * i, 0))
            pass
        return image
    pass


class Unshredder(object):
    "recover shredded image"
    def __init__(self, in_image, shred_width, evaluator=None):
        self.in_image = in_image
        self.shred_width = shred_width
        self.shred_count = in_image.size[0] // shred_width
        self.height = in_image.size[1]
        self.evaluator = evaluator or Closeness.by_hsv_hist
        pass

    def run(self):
        parts = []
        for i in range(self.shred_count):
            # PIL box should be (l, t, r, b) as ranges: [l, r) and [t, b)
            box = (self.shred_width * i, 0, 
                   self.shred_width * (i + 1), self.height)
            parts.append(Part([self.in_image.crop(box)], self.shred_width))
            pass
        while len(parts) > 1:
            left, right = self.pick_nearest(parts)
            parts = [part for part in parts if part != left and part != right]
            parts.append(left.join(right))
            pass
        assert len(parts) == 1
        return parts[0].to_image()

    def pick_nearest(self, parts):
        assert len(parts) > 1
        queue = []
        for a, b in itertools.combinations(parts, 2):
            queue.append((self.evaluate(a, b), a, b))
            queue.append((self.evaluate(b, a), b, a))
            pass
        assert len(queue) >= 2
        closeness, left, right = min(queue, key=lambda tup: tup[0])
        return (left, right)

    def evaluate(self, left, right):
        return self.evaluator(left.rights, right.lefts, self.height)
    pass


class Closeness(object):
    "collection of closeness methods"
    @classmethod
    def by_rgb_ave(cls, lcolors, rcolors, size):
        # rgb diff average: bad result
        return sum(Math.vect_dist(lp, rp, 3) 
                   for lp, rp in zip(lcolors, rcolors)) 

    @classmethod
    def by_hsv_ave(cls, lcolors, rcolors, size):
        # hsv diff average: good result
        # (lighting value similarity may be important)
        return sum(Math.vect_dist(Math.hsv(lp), Math.hsv(rp), 3) 
                   for lp, rp in zip(lcolors, rcolors)) 

    @classmethod
    def by_rgb_med(cls, lcolors, rcolors, size):
        # rgb diff median: good result
        # (may ignore untypical points)
        r = [Math.vect_dist(lp, rp, 3) 
             for lp, rp in zip(lcolors, rcolors)]
        r.sort()
        return r[len(r) // 2]

    @classmethod
    def by_rgb_hist(cls, lcolors, rcolors, size):
        # histogram distance of rgb: bad result
        ls = [Math.bs2fs(c) for c in lcolors]
        rs = [Math.bs2fs(c) for c in rcolors]
        return Math.hist_dist(ls, rs, 64, 3)

    @classmethod
    def by_hsv_hist(cls, lcolors, rcolors, size):
        # histogram distance of hsv: good result(16, 64)
        ls = [Math.hsv(c) for c in lcolors]
        rs = [Math.hsv(c) for c in rcolors]
        return Math.hist_dist(ls, rs, 64, 3)
    pass


class ShredSize(object):
    "Guess shred size"
    def __init__(self, image, evaluator=None):
        self.image = image
        self.width = image.size[0]
        self.height = image.size[1]
        self.lines = [[self.image.getpixel((x, y)) 
                       for y in range(image.size[1])]
                      for x in range(image.size[0])]
        self.evaluator = evaluator or Closeness.by_hsv_hist
        pass

    def guess(self):
        minshred = max(self.width // 100, 2)
        dists = [(self.evaluate(rx - 1, rx), rx)
                 for rx in range(1, self.width)]
        dists.sort(key=lambda t: t[0], reverse=True)
        assert len(dists) > 0
        size = dists[0][1]
        for i in range(1, len(dists)):
            val = Math.gcd(size, dists[i][1])
            if val < minshred: 
                break
            size = val
            pass
        return size

    def evaluate(self, lx, rx):
         return self.evaluator(self.lines[lx], self.lines[rx], self.height)
    pass


class Math(object):
    "misc math functions"
    @classmethod
    def bs2fs(cls, bs):
        assert all(0 <= v and v < 256 for v in bs)
        return tuple(v / 255.0 for v in bs)

    @classmethod
    def hsv(cls, rgba):
        assert all(0 <= v and v < 256 for v in rgba)
        frgb = cls.bs2fs(rgba)
        return colorsys.rgb_to_hsv(frgb[0], frgb[1], frgb[2])

    @classmethod
    def log2(cls, n):
        assert n >= 0
        r = 0
        p2 = 1
        while p2 < n:
            r += 1
            p2 = p2 * 2
            pass
        return r

    @classmethod
    def gcd(cls, a, b):
        assert a >= 0 and b >= 0
        while a != b:
            if a > b: a = a - b
            if b > a: b = b - a
            pass
        return a

    @classmethod
    def hist(cls, vects, histsize, paramsize):
        m = histsize - 1
        hist = [[0] * histsize for k in range(paramsize)]
        for v in vects:
            for k in range(paramsize):
                hist[k][int(v[k] * m)] += 1
                pass
            pass
        return hist

    @classmethod
    def vect_dist(cls, va, vb, size):
        return sum((va[i] - vb[i]) ** 2 for i in range(size))

    @classmethod
    def vect_log_dist(cls, va, vb, size):
        return sum((cls.log2(va[i]) - cls.log2(vb[i])) ** 2 
                   for i in range(size))

    @classmethod
    def hist_dist(cls, va, vb, histsize, paramsize):
        ha = cls.hist(va, histsize, paramsize)
        hb = cls.hist(vb, histsize, paramsize)
        return sum(cls.vect_log_dist(ha[k], hb[k], histsize)
                   for k in range(paramsize))
    pass


if __name__ == "__main__":
    main()
    pass




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
