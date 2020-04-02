---
title: PIL example video file clip (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'video file clip'

Functions in program: 
* `def resizer(pic, newsize):`
* `def pil_rotater(pic, angle, resample, expand):`

Modules used in program: 
* `import numpy as np`

## python video file clip

Python PIL example: video file clip

```python
from moviepy.video.io.VideoFileClip import VideoFileClip as VFC
from PIL import Image
import numpy as np


PIL_FOUND = True


def pil_rotater(pic, angle, resample, expand):
    return np.array(Image.fromarray(pic).rotate(angle, expand=expand,
                                                resample=resample))


def resizer(pic, newsize):
    newsize = list(map(int, newsize))[::-1]
    shape = pic.shape
    if len(shape) == 3:
        newshape = (newsize[0], newsize[1], shape[2])
    else:
        newshape = (newsize[0], newsize[1])

    pilim = Image.fromarray(pic)
    resized_pil = pilim.resize(newsize[::-1], Image.ANTIALIAS)
    # arr = np.fromstring(resized_pil.tostring(), dtype='uint8')
    # arr.reshape(newshape)
    return np.array(resized_pil)


resizer.origin = "PIL"


class VideoFileClip(VFC):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)

    def rotate(clip, angle, unit='deg', resample="bicubic", expand=True):
        resample = {"bilinear": Image.BILINEAR,
                    "nearest": Image.NEAREST,
                    "bicubic": Image.BICUBIC}[resample]

        if not hasattr(angle, '__call__'):
            # if angle is a constant, convert to a constant function
            a = +angle
            angle = lambda t: a

        transpo = [1, 0] if clip.ismask else [1, 0, 2]

        def fl(gf, t):

            a = angle(t)
            im = gf(t)
            if unit == 'rad':
                a = 360.0 * a / (2 * np.pi)
            if (a == 90) and expand:
                return np.transpose(im, axes=transpo)[::-1]
            elif (a == -90) and expand:
                return np.transpose(im, axes=transpo)[:, ::-1]
            elif (a in [180, -180]) and expand:
                return im[::-1, ::-1]
            elif not PIL_FOUND:
                raise ValueError('Without "Pillow" installed, only angles 90, -90,'
                                 '180 are supported, please install "Pillow" with'
                                 "pip install pillow")
            else:
                return pil_rotater(im, a, resample=resample, expand=expand)

        return clip.fl(fl, apply_to=["mask"])

    def resize(clip, newsize=None, height=None, width=None, apply_to_mask=True):
        w, h = clip.size

        if newsize is not None:

            def trans_newsize(ns):
                if isinstance(ns, (int, float)):
                    return [ns * w, ns * h]
                else:
                    return ns

            if hasattr(newsize, "__call__"):
                newsize2 = lambda t: trans_newsize(newsize(t))
                if clip.ismask:
                    fun = lambda gf, t: (1.0 * resizer((255 * gf(t)).astype('uint8'),
                                                       newsize2(t)) / 255)
                else:
                    fun = lambda gf, t: resizer(gf(t).astype('uint8'),
                                                newsize2(t))

                return clip.fl(fun, keep_duration=True,
                               apply_to=(["mask"] if apply_to_mask else []))

            else:
                newsize = trans_newsize(newsize)
        elif height is not None:
            if hasattr(height, "__call__"):
                fun = lambda t: 1.0 * int(height(t)) / h
                return clip.resize(clip, fun)
            else:
                newsize = [w * height / h, height]

        elif width is not None:
            if hasattr(width, "__call__"):
                fun = lambda t: 1.0 * width(t) / w
                return clip.resize(clip, fun)

            newsize = [width, h * width / w]

        # From here, the resizing is constant (not a function of time), size=newsize

        if clip.ismask:
            fl = lambda pic: 1.0 * resizer((255 * pic).astype('uint8'), newsize) / 255.0
        else:
            fl = lambda pic: resizer(pic.astype('uint8'), newsize)

        newclip = clip.fl_image(fl)

        if apply_to_mask and clip.mask is not None:
            newclip.mask = clip.resize(clip.mask, newsize, apply_to_mask=False)

        return newclip


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
