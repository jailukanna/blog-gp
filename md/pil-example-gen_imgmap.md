---
title: pil example gen imgmap (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'gen imgmap'

Functions in program: 
* `def main(argv=None):`
* `def main_(base_img, attrs_csv, output_file="out.html", integer_colors=False):`
* `def timeit(func):`
* `def generate_area_tags(masks, attrs):`
* `def generate_masks(base_img, attrs, integer_colors=False):`
* `def process_csv(csv_file):`
* `def rgbtup(rgb):`
* `def rgbhex(color):`
* `def rgbint(color):`
* `def areatags_from(mask_path=None, kwds={}):`
* `def fill_holes(shp):`
* `def flip_lonely_pixels(box, min_neighbors=2):`
* `def simplified(poly, min_area = 0.25):`
* `def area_of(yx_1, yx_2, yx_3):`
* `def poly_from(shp):`
* `def shapes_from(a):`
* `def neighbors_of(yx, corners=True):`
* `def count_neighbors(yx, val, img):`
* `def fill(img, yx, val):`
* `def dist(yx1, yx2):`
* `def combinations_of(A,B):`
* `def pixels_of(img):`
* `def bit2rgb(b):`
* `def save(a, name):`

Modules used in program: 
* `import Image`
* `import csv`
* `import getopt`
* `import os`
* `import time`
* `import sys`

## python gen imgmap

Python pil example: gen imgmap

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

## changelog
# Original version by Alex Ross
# patch to validate html, fix hex parsing, and check PIL version by micheal -- 10/16/2007
# http://web.archive.org/web/20081025072718/http://cosmotron.org/2007/01/generating-html-image-maps.html

"""gen_imgmap.py

Usage: gen_imgmap.py [-hi] image_file csv_file out_file
Options:
 -i, --integer-colors     Interpret color values as integers (hex is default).
 -h, --help               Show this information.

Arguments:
 image_file – png with one color per clickable area.  The areas do not
              have to be contiguous, and may have holes.
 csv_file   – csv mapping colors to tag attributes.  The first row must be
              labels. The first column must be either a hexadecimal color, or
              an integer color (specify -i for integer colors).  The remaining
              columns will be attached to the area tags as attribute values
              (theattribute key will be the column label).
 out_file   – file name to which the generated image map will be written.

Requirements:
 ImageMagick ≥ 6.3.0
 Python ≥ 2.4
 numpy ≥ 1.0
 PIL ≥ 1.1.6\
"""

import sys
import time
import os
import getopt
import csv
import Image
from numpy import *

pil_version = Image.VERSION.split('.')
pil_version =[int(e) for e in pil_version]
if not (len(pil_version) == 3 and pil_version[0] >= 1 and pil_version[1] >= 1 and pil_version[2] >= 6):
    sys.stderr.write("your PIL version is too old %s minimum 1.1.6. needed\n" % Image.VERSION)
    sys.exit(1)

sys.stdout = sys.stderr

def save(a, name):
    Image.fromarray(bit2rgb(a)).save(name)

def bit2rgb(b):
    """ Convert a bitmap to an RGB img. """
    b = b[:,:,newaxis]*255
    return concatenate((b, b, b), 2)

def pixels_of(img):
    for y, x in combinations_of(range(img.shape[0]), range(img.shape[1])):
        yield y,x

def combinations_of(A,B):
    for a in A:
        for b in B:
            yield a,b

def dist(yx1, yx2):
    """ Euclidean distance from `yx1` to `yx2`."""
    y1, x1 = yx1
    y2, x2 = yx2
    return sqrt(float((x2-x1)**2 + (y2-y1)**2))

def fill(img, yx, val):
    """ Fill `img` from `yx` with `val`. """
    img = img.copy()
    edge = [yx]
    while edge:
        newedge = []
        for (y,x) in edge:
            for (s, t) in ((y, x+1), (y, x-1), (y+1, x), (y-1, x)):
                try:
                    p = img[s, t]
                except IndexError:
                    pass
                else:
                    if p != val:
                        img[s, t] = val
                        newedge.append((s, t))
        edge = newedge
    return img

def count_neighbors(yx, val, img):
    """ Return the number of pixels adjacent to (Y,X) with value `val`. """
    c = 0
    for t,s in neighbors_of(yx, corners=False):
        if (t < 0 or t >= img.shape[0]) or (s < 0 or s >= img.shape[1]):
            c+=1
            continue
        if img[t,s] == val:
            c+=1
    return c

def neighbors_of(yx, corners=True):
    """ Yield the coordinates of pixels that are adjacent yx.
       
        If `corners` is False, don't yield the pixels that are adjacent
        to the corners of `yx`.
    """
    y,x = yx
    if corners:
        pixels = [(y-1, x-1), (y-1, x), (y-1, x+1),
                  (  y, x-1),           (  y, x+1),
                  (y+1, x-1), (y+1, x), (y+1, x+1)]
    else:
        pixels = [(y-1, x), (  y, x+1), (y+1, x), (  y, x-1),]
    for yx in pixels:
        yield yx

def shapes_from(a):
    """ Yield the separate contiguous shapes of a. """
    a = a.copy()
    while True:
        Y, X  = a.nonzero()
        YX = zip(Y,X)
        y,x = min(YX)
        b = fill(a, (y,x), 0)
        s = logical_xor(b, a)
        yield s
        a = logical_xor(s,a)
        if not a.any():
            break

def poly_from(shp):
    """ Return the bounding polygon of a contiguous, solid shp.
    
        `shp` should be a binary numpy array.  All parts of the shape should be
        connected to all other parts of the shape (contiguous).  There should not
        be a hole in any part of the shape (solid).
    """
    if not shp.any():
        return []
    Y, X  = shp.nonzero()
    YX    = zip(Y,X)
    y,x = min(YX)
    wfirst = (y, x)
    bfirst = (y-1, x)
    hist = [(wfirst, bfirst)]
    wcurs, bcurs = wfirst, bfirst
    # Traverse the shape in a clockwise manner, recording all
    # pixels as we go.
    while True:
        wn = [yx for yx in list(neighbors_of(wcurs))+[wcurs] if yx in YX]
        bn = [yx for yx in list(neighbors_of(bcurs)) + [bcurs] if not yx in YX]
        distances = [dist(w, b) for w, b in combinations_of(wn, bn)]
        temp = sorted(zip(distances, list(combinations_of(wn, bn))))
        next_curs = [wb for d, wb in temp if d < 2.0 and wb not in hist]
        if all([w in [wh for wh, bh in hist if dist(b, bh) < 2] for w,b in next_curs]) or not next_curs:
            break
        for wb in next_curs:
            wcurs, bcurs = wb
            hist.append(wb)
            break
    poly = [w for w,b in hist]
    return poly

def area_of(yx_1, yx_2, yx_3):
    """ Euclidean area of the triangle `yx_1`, `yx_2`, `yx_3`. """
    y1, x1 = yx_1
    y2, x2 = yx_2
    y3, x3 = yx_3
    return 1/2. * abs(-x2*y1 + x3*y1 + x1*y2 - x3*y2 - x1*y3 + x2*y3)

def simplified(poly, min_area = 0.25):
    """ Return a simplified version of `poly` (`poly` is a list or array of points.).
    
        Uses a “triangle-based” simplification algorithm.  For consecutive
        points along the edge of `poly`"""
    # simplify polygon
    i = 0
    while True:
        if i+2 >= len(poly):
            break
        j,k,m = i, i+1, i+2
        if area_of(poly[j], poly[k], poly[m]) < min_area:
            del poly[k]
            i = 0
            continue
        else:
            i+=1
    return poly

def flip_lonely_pixels(box, min_neighbors=2):
    """ Fill in any lonely pixels (a pixel is lonely 
        if it has less than 2 identical neighbors).
    """
    print(" Flipping lonely pixels…")
    box = box.copy()
    while True:
        boxl = box.copy()
        for Y, X in pixels_of(box):
            if count_neighbors((Y,X), box[Y,X], box) < min_neighbors:
                box[Y,X] = not box[Y,X]
        if (boxl == box).all():
            break
    return box

def fill_holes(shp):
    """ Return `shp` with all of its holes filled in."""
    e = shp.copy()
    for yx in [(0,0), (-1,0), (0,-1), (-1,-1)]:
        d = fill(shp, yx, True)
        e = logical_or(e, d)
    if logical_not(e).any():
        # the shape has a hole
        z_order = -1
    else:
        z_order = 1
    filled = logical_or(shp, logical_not(e))
    save(filled, 'filled.png')
    return filled, z_order


def areatags_from(mask_path=None, kwds={}):
    """ Return a list of html area tags for the mask at `mask_path`.
        
        Any kwds is a dictionary of attributes which are added to
        ALL of the returned tags.  I.e. if you pass {href="http://google.com"},
        all of the area tag will have “href="http://google.com"”.
    """
    img = asarray(Image.open(mask_path))
    img = img[:,:,0].astype(bool)
    # get the smallest box containing the shape in a.
    if not img.any():
        return []
    Y, X = img.nonzero()
    miny, maxy = min(Y), max(Y)+1
    minx, maxx = min(X), max(X)+1
    pad = 3
    box = zeros((maxy-miny + 2*pad, maxx - minx + 2*pad), bool)
    box[pad:-pad, pad:-pad] = img[miny:maxy, minx:maxx]
    box = flip_lonely_pixels(box)
    if not box.any():
        return []
    # find polys
    print(" Finding shapes…",)
    shapes = []
    for i, shp in enumerate(shapes_from(box)):
        print(i+1,)
        shp, z_order = fill_holes(shp)
        shapes.append((shp, z_order))
    print
    print(" Computing bounding polygons from shapes…")
    polys = []
    for i, (shp, z_order) in enumerate(shapes):
        print("   %i:   z_order = %i" % (i, z_order))
        polys.append((poly_from(shp), z_order))
    # simplify polys
    print(" Simplifying polys…")
    # two passes of simplification.
    polys = [(simplified(poly, min_area=0.8), z_order) for poly, z_order in polys]
    polys = [(simplified(poly[len(poly)/2:] + poly[:len(poly)/2], min_area=0.8), z_order) for poly, z_order in polys]
    polys = [([(miny + y - pad + 2, minx + x - pad + 1) for y,x in poly], z_order) for poly, z_order in polys]
    # format html
    tags = []
    print(" Formatting area tag…")
    for poly, z_order in polys:
        tag = []
        tag.append('<area shape="poly" ')
        for key in kwds.keys():
            tag.append('%s="%s" ' % (key, kwds[key]))
        tag.append('coords="')
        tag.append(", ".join("%i,%i" % (x,y) for y,x in poly))
        tag.append('"></area>\n')
        tags.append(("".join(tag), z_order))
    return tags

def rgbint(color):
    """ Converts an integer color code into an rgb tuple. """
    color = int(color) - 2**25
    return (color % 2**8, color % 2**16 / 2**8, color / 2**16)

def rgbhex(color):
    """ Convert a hexadecimal color code into a rgb tuple. """
    _2hex = lambda n: int(n,16)
    if len(color) in [4,7]:
        assert color[0] == "#"
        color = color[1:]
    if len(color) == 6:
        s = [color[i:i+2] for i in range(0,5,2)]
    else:
        s = [color[i] for i in range(0,3)]
    return tuple(map(_2hex, s))

def rgbtup(rgb):
    r,g,b = rgb
    return 2**25 + b * 2**16 + g * 2**8 + r

def process_csv(csv_file):
    """ Generate a dictionary keyed by color from a csv. """
    import csv as _csv
    f = open(csv_file)
    csv = _csv.reader(f)
    # the first line should be the labels
    attrs = {}
    labels = csv.next()
    labels = [lbl.lower() for lbl in labels]
    assert "color" == labels[0]
    for row in csv:
        if row:
            color = row[0]
            attrs[color] = []
            for x, b in enumerate(labels[1:]):
                attrs[color].append((b, row[x+1].strip(" \"")))
    return attrs

def generate_masks(base_img, attrs, integer_colors=False):
    masks = []
    if not os.path.exists("./masks"):
        os.mkdir("masks")
    for color in attrs:
        if integer_colors:
            rgb = rgbint(color)
        else:
            rgb = rgbhex(color)
        label, value = attrs[color][0]
        new_mask = "masks/%s.png" % value
        if not os.path.exists(new_mask):
            ex = 'convert %s -transparent "rgb%r" '\
                 '-fx "a" +matte -negate %s' % (base_img, rgb, new_mask)
            print(ex)
            os.system(ex)
        masks.append([color, new_mask])
    return masks

def generate_area_tags(masks, attrs):
    area_tags = []
    for color, m in masks:
        print("Processing area tag(s) for %s…" % m)
        tags = areatags_from(m, dict(attrs[color]))
        if not tags:
            continue
        for tag, z_order in tags:
            if z_order > 0:
                area_tags.insert(0, tag)
            else:
                area_tags.append(tag)
    return area_tags

def timeit(func):
    def wrapper(*args, **kwds):
        begin_time = time.time()
        result = func(*args, **kwds)
        end_time = time.time()
        print("Total time: %f minutes." % ((end_time - begin_time) / 60))
        return result
    return wrapper

@timeit
def main_(base_img, attrs_csv, output_file="out.html", integer_colors=False):
    attrs = process_csv(attrs_csv)
    masks = generate_masks(base_img, attrs, integer_colors)
    area_tags = generate_area_tags(masks, attrs)
    try:
        html = file(output_file, "w")
        html.write('<html><body>\n<map name="genmap">\n')
        for tag in area_tags:
            html.write(tag)
        html.write("</map>\n")
        html.write('<img src="%s" usemap="#genmap">\n' % base_img)
        html.write("</body></html>\n")
    finally:
        html.close()

class Usage(Exception):
    def __init__(self, msg):
        self.msg = msg

def main(argv=None):
    if argv is None:
        argv = sys.argv
    try:
        try:
            opts, args = getopt.getopt(argv[1:], "hi", ["help", "integer-colors"])
        except getopt.error, msg:
             raise Usage(msg)
        # more code, unchanged
        integer_colors = False
        for opt in opts:
            if opt[0] in ["-h", "--help"]:
                print(__doc__)
                return 2
            elif opt[0] in ["-i", "--integer-colors"]:
                integer_colors = True
        if len(args) not in [2,3] :
            raise Usage("Invalid arguments.")
            return 2
        else:
            base_img, attrs_csv, out_file = args
        main_(base_img, attrs_csv, out_file, integer_colors)
    except Usage, err:
        print(>>sys.stderr, err.msg)
        print(>>sys.stderr, "for help use --help")
        return 2

if __name__ == "__main__":
    sys.exit(main())

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
