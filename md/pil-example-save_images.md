---
title: pil example save images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'save images'

Functions in program: 
* `def make_directory_if_not_exists(directory):`
* `def save_pil_image(size, color=COLOR, directory=DIRECTORY, quality=JPEG_SAVE_QUALITY):`

Modules used in program: 
* `import os`

## python save images

Python pil example: save images

```python
import os

from PIL import Image


IMAGE_SIZES = range(500, 20000, 2000)
JPEG_SAVE_QUALITY = 100
DIRECTORY = '~/Desktop/temp/'
COLOR = 'red'

def save_pil_image(size, color=COLOR, directory=DIRECTORY, quality=JPEG_SAVE_QUALITY):
    directory = os.path.expanduser(directory)
    file_name = str(size) + '.jpg'
    path = os.path.join(directory, file_name)

    make_directory_if_not_exists(directory)

    image = Image.new('RGB', (size, size,), color)

    with open(path, 'w+') as file:
        image.save(path, format='JPEG', quality=quality)
    print('saved ' + path)

def make_directory_if_not_exists(directory):
    try: 
        os.makedirs(directory)
    except OSError:
        if not os.path.isdir(directory):
            raise

for size in IMAGE_SIZES:
    save_pil_image(size)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
