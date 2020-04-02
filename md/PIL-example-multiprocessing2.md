---
title: PIL example multiprocessing2 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'multiprocessing2'

Functions in program: 
* `def create_thumbnail(filename): `
* `def get_image_paths(folder):`

Modules used in program: 
* `import PIL `
* `import os `

## python multiprocessing2

Python PIL example: multiprocessing2

```python
import os 
import PIL 
 
from multiprocessing import Pool 
from PIL import Image
 
SIZE = (75,75)
SAVE_DIRECTORY = 'thumbs'
 
def get_image_paths(folder):
    return (os.path.join(folder, f) 
            for f in os.listdir(folder) 
            if 'jpeg' in f)
 
def create_thumbnail(filename): 
    im = Image.open(filename)
    im.thumbnail(SIZE, Image.ANTIALIAS)
    base, fname = os.path.split(filename) 
    save_path = os.path.join(base, SAVE_DIRECTORY, fname)
    im.save(save_path)
 
if __name__ == '__main__':
    folder = os.path.abspath(
        '11_18_2013_R000_IQM_Big_Sur_Mon__e10d1958e7b766c3e840')
    os.mkdir(os.path.join(folder, SAVE_DIRECTORY))
 
    images = get_image_paths(folder)
 
    pool = Pool()
        pool.map(create_thumbnail,images)
        pool.close()
        pool.join()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
