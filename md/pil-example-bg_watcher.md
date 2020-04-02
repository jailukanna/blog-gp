---
title: pil example bg watcher (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'bg watcher'

Functions in program: 
* `def cropped_thumbnail(img, size):`
* `def flat( *nums ):`

Modules used in program: 
* `import os`

## python bg watcher

Python pil example: bg watcher

```python
import os
from string import Template
from PIL import Image
from PIL.ImageStat import Stat
from PIL.ImageFilter import GaussianBlur
from PIL import ImageEnhance
from time import sleep

def flat( *nums ):
    'Build a tuple of ints from float or integer arguments. Useful because PIL crop and resize require integer points.'
    return tuple(int(round(n)) for n in nums)

class Size(object):
    def __init__(self, pair):
        self.width = float(pair[0])
        self.height = float(pair[1])

    @property
    def aspect_ratio(self):
        return self.width / self.height

    @property
    def size(self):
        return flat(self.width, self.height)

def cropped_thumbnail(img, size):
    '''
    Builds a thumbnail by cropping out a maximal region from the center of the original with
    the same aspect ratio as the target size, and then resizing. The result is a thumbnail which is
    always EXACTLY the requested size and with no aspect ratio distortion (although two edges, either
    top/bottom or left/right depending whether the image is too tall or too wide, may be trimmed off.)
    '''
    original = Size(img.size)
    target = Size(size)

    if target.aspect_ratio > original.aspect_ratio:
        # image is too tall: take some off the top and bottom
        scale_factor = target.width / original.width
        crop_size = Size( (original.width, target.height / scale_factor) )
        top_cut_line = (original.height - crop_size.height) / 2
        img = img.crop( flat(0, top_cut_line, crop_size.width, top_cut_line + crop_size.height) )
    elif target.aspect_ratio < original.aspect_ratio:
        # image is too wide: take some off the sides
        scale_factor = target.height / original.height
        crop_size = Size( (target.width/scale_factor, original.height) )
        side_cut_line = (original.width - crop_size.width) / 2
        img = img.crop( flat(side_cut_line, 0,  side_cut_line + crop_size.width, crop_size.height) )

    return img.resize(target.size, Image.ANTIALIAS)


with os.popen('dconf watch /org/mate/desktop/background/picture-filename') as pipe:
    for line in pipe:
        value = line.strip()

        if not value.startswith("'/home"):
            continue

        path = value[1:-1] # strip quotes
        image = Image.open(path)
        image = cropped_thumbnail(image, (3840, 2160))

        bottom = image.crop((1200, 2160 - 64, 2640, 2160))
        rgb = Stat(bottom).mean[:3]
        dark = tuple([int(comp / 1.3) for comp in rgb])
        border = tuple([int(comp / 1.1) for comp in dark])
        dark_s = ';;'.join([str(comp) for comp in dark])
        border_s = ';;'.join([str(comp) for comp in border])

        top = image.crop((0, 0, 3840, 24))
        top_blurred = top.filter(GaussianBlur(100))
        enhancer = ImageEnhance.Brightness(top_blurred)
        top_blurred = enhancer.enhance(0.5)
        os.popen('dconf write /org/mate/panel/toplevels/top/background/type "\'none\'"')
        top_blurred.save(os.path.expanduser('~/Pictures/DE/panel.png'))
        sleep(1)
        os.popen('dconf write /org/mate/panel/toplevels/top/background/type "\'image\'"')
        #  rgb = Stat(top).mean
        #  panel = tuple([int(comp / 2.0) for comp in rgb])
        #  panel_s = ','.join([str(comp) for comp in dark])

        with open(os.path.expanduser('~/.local/share/plank/themes/dt-2/template')) as template_fd:
            template = Template(template_fd.read())

        new_theme = template.substitute(
            dark_s=dark_s,
            border_s=border_s
        )

        with open(os.path.expanduser('~/.local/share/plank/themes/dt-2/dock.theme'), 'w') as theme_fd:
            theme_fd.write(new_theme)

        #  os.popen('dconf write /org/mate/panel/toplevels/top/background/color "\'rgb({})\'"'.format(
        #      panel_s
        #  ))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
