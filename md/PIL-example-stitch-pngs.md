---
title: PIL example stitch-pngs (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'stitch-pngs'


Modules used in program: 
* `import fnmatch`
* `import os`

## python stitch-pngs

Python PIL example: stitch-pngs

```python
import os
import fnmatch
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

dirs = filter(os.path.isdir, os.listdir(os.curdir))
has_files = filter(lambda this_dir: os.listdir(this_dir), dirs) # Get all directories that actually contain files.
count_pngs = len(filter(lambda this_file: this_file.endswith('.png'), os.listdir(has_files[0])))

try:
    font = ImageFont.truetype('/System/Library/Fonts/Monaco.dfont', 14) 
    # ^ You'll probably want to change this to another font (and probably don't need to use the path) if you're not on a Mac.
except IOError:
    font = ImageFont.load_default()

for i in range(count_pngs):
    wildcard_path = '*_' + str(i).zfill(2) + '.png' # you'll need to change this if your files don't end in "_00.png", "_01.png", "_02.png", etc.
    images = list(map(lambda j: Image.open(os.path.join(j, fnmatch.filter(os.listdir(j), wildcard_path)[0])), has_files))
    image_dimensions = {'width': images[0].size[0], 'height': images[0].size[1]}
    image_height_plus_textspace = image_dimensions['height'] + 20
    new_image = Image.new('RGBA', (image_dimensions['width']*len(has_files), image_height_plus_textspace), color='black')
    offset = 5

    for v in range(len(images)):
        draw = ImageDraw.Draw(new_image)
        new_image.paste(im=images[v], box=(image_dimensions['width'] * v, 0))
        draw.text((image_dimensions['width'] * v + offset, image_dimensions['height']), has_files[v], (255,255,255), font=font)
    new_image.save('result_' + str(i).zfill(4) + '.png')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
