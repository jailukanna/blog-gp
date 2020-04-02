---
title: pil example match-colours (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'match-colours'

Functions in program: 
* `def usage():`
* `def main(source_images, target_images):`
* `def amplify_channels(image, factors):`
* `def match_colours(source_image, target_image):`

Modules used in program: 
* `import ImageStat`
* `import Image`

## python match-colours

Python pil example: match-colours

```python
#!/usr/bin/env python2

# requires PIL (python-imaging)

import Image
import ImageStat

from sys import argv, exit
from os.path import basename, splitext

MODIFIED_SUFFIX = "_mod.jpg"

def match_colours(source_image, target_image):
    """
    Calculate per-channel amplification to match colours of the target_file.

    Return (x_red, x_green, x_blue).
    """
    s = ImageStat.Stat(source_image)
    t = ImageStat.Stat(target_image)
    k_chans = [ (tc/sc if sc else 1.0) for sc, tc in zip(s.mean, t.mean) ]
    return tuple(k_chans)

def amplify_channels(image, factors):
    """
    Apply per-channel amplification to the image.
    
    image    PIL image
    factors  a tuple of amplification coefficients, one per channel

    Return a new PIL image.
    """
    chans = image.split()
    chans2 = [ Image.eval(c, lambda x: x*f) for c,f in zip(chans, factors)]
    image2 = Image.merge(image.mode, chans2)
    return image2

def main(source_images, target_images):
    # calculate average transform coefficients using all image pairs
    avg_multipliers = []
    n = len(zip(source_images, target_images))  # number of pairs
    for s, t in zip(source_images, target_images):
        simg = Image.open(s) 
        simg.load()  # workaround for http://bugs.debian.org/561965
        timg = Image.open(t)
        multipliers = match_colours(simg, timg)
        ms = [ float(m)/n for m in multipliers ]
        if not avg_multipliers:
            avg_multipliers = [ float(m)/n for m in multipliers ]
        else:
            avg_multipliers = [ old_avg+float(m)/n
                                for old_avg, m
                                in zip(avg_multipliers, multipliers)]
    # transform and save every source image
    for s in source_images:
        simg = Image.open(s)
        simg.load()  # workaround for http://bugs.debian.org/561965
        modified = amplify_channels(simg, avg_multipliers)
        out_filename = splitext(s)[0] + MODIFIED_SUFFIX
        modified.save(out_filename)

def usage():
    help = \
    """%s bad_image_1 bad_image_2 ... good_image_1 good_image_2 ...

    Compare bad and good images pairwise, calculate per-channel
    amplification coefficients to match colours in average, adjust bad
    images and save them to new files appending "%s" to the filename.
    """
    print(help % (basename(argv[0]), MODIFIED_SUFFIX))

if __name__ == "__main__":
    args = argv[1:]
    if not args or "-h" in args or "--help" in args:
        usage()
        exit(0)
    else:
        n = len(args)
        bad_imgs = args[0:n//2]
        good_imgs = args[n//2:]
        main(bad_imgs, good_imgs)
        


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
