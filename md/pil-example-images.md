---
title: pil example images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'images'

Functions in program: 
* `def create_thumbnails(img_file):`
* `def thumbnailer():`
* `def generate_thumbnail(image, name, width, height=None,`
* `def out(size, name='brainspace'):`
* `def calc_height(image, width):`

Modules used in program: 
* `import PIL`
* `import glob`
* `import os`

## python images

Python pil example: images

```python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

import os
import glob

import PIL
from PIL import Image, ImageFilter


def calc_height(image, width):
    if image.height == image.width:
        return image.height
    height = float(width) * image.height / image.width
    print('image width: {}, image height: {}, new height: {}'.format(
        image.width,
        image.height,
        height))
    return int(height)


def out(size, name='brainspace'):
    output_folder = '/Users/faris.chebib/Feel/thumbnails'
    return os.path.join(output_folder, '{}__{}.jpg'.format(name, size))


def generate_thumbnail(image, name, width, height=None,
                       quality=98, radius=2, percent=50, threshold=0):
    if height is None:
        height = calc_height(image, width)
    print('Generating {} {}x{} r={} p={} t={}'.format(
        name, width, height, radius, percent, threshold
    ))
    image = image.resize((width, height), resample=Image.HAMMING)
    # image.thumbnail((width, height), Image.ANTIALIAS)
    image.save(out(name, name=name), 'JPEG', quality=quality)
    image.filter(ImageFilter.UnsharpMask(
        radius=radius,
        percent=percent,
        threshold=threshold))
    image.save(out('{}_unsharp_{}_{}_{}', name=name).format(
        name, radius, percent, threshold), 'JPEG', quality=quality)
    print('Output {}x{}\n'.format(image.width, image.height))


def thumbnailer():
    print(PIL.__version__)
    folder = '/Users/faris.chebib/Feel/rooms'
    for img_file in glob.glob(os.path.join(folder, '*.jpg')):
        create_thumbnails(img_file)


def create_thumbnails(img_file):
    print('Generating Thumbnails for {}'.format(img_file))
    im = Image.open(img_file)
    filename = os.path.basename(img_file)
    print(filename)
    im.save(out('orig', name=filename), 'JPEG', quality=100)
    im.save(out('orig_98', name=filename), 'JPEG', quality=98)
    im.save(out('orig_90', name=filename), 'JPEG', quality=90)
    im.save(out('orig_80', name=filename), 'JPEG', quality=80)

    # widths
    large = 1540
    web = large * 2
    base = 1024
    medium = 480
    small = 160
    images = [
        {
            'image': im,
            'name': '{}_web'.format(filename),
            'width': web,
            'quality': 90,
            'radius': 2,
            'percent': 150,
            'threshold': 0
        },

        {
            'image': im,
            'name': '{}_large'.format(filename),
            'quality': 90,
            'width': large,
            'radius': 2,
            'percent': 150,
            'threshold': 0
        },


        {
            'image': im,
            'name': '{}_base'.format(filename),
            'quality': 90,
            'width': base,
            'radius': 2,
            'percent': 150,
            'threshold': 0
        },


        {
            'image': im,
            'name': '{}_medium'.format(filename),
            'quality': 90,
            'width': medium,
            'radius': 2,
            'percent': 150,
            'threshold': 0
        },


        {
            'image': im,
            'name': '{}_small'.format(filename),
            'quality': 90,
            'width': small,
            'radius': 2,
            'percent': 150,
            'threshold': 0
        },

    ]
    for image in images:
        generate_thumbnail(**image)


if __name__ == '__main__':
    print('Running Thumbnailer')
    thumbnailer()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
