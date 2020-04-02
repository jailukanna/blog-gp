---
title: PIL example image crop (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image crop'


Modules used in program: 
* `import os`
* `import glob`

## python image crop

Python PIL example: image crop

```python
import glob
import os
from PIL import Image

IMAGE_LENGTH = 256

artists = ['jean-auguste-dominique-ingres', 'gustave-courbet', 'paul-cezanne', 'georges-seurat', 'henri-matisse', 'kazimir-malevich', 'andy-warhol', 'frank-stella']

for artist in artists:
    print(artist)
    in_dir  = './' + artist
    out_dir =  './resized/' + artist + '-resized'
    
    if not os.path.exists(out_dir):
        os.makedirs(out_dir)
    
    files = glob.glob(in_dir + '/*')
    
    for in_file in files:
        file_name = in_file.rsplit('/', 1)[1]
        out_file = out_dir + '/' + file_name
        
        image = Image.open(in_file)
        
        # Get dimensions
        width, height = image.size
        c_x, c_y = width * 0.5, height * 0.5
        
        length = height if width >= height else width
        
        left = (width - length) * 0.5
        top = (height - length) * 0.5
        right = (width + length) * 0.5
        bottom = (height + length) * 0.5
        
        image = image.crop((left, top, right, bottom))
        
        # resize
        image.resize((IMAGE_LENGTH, IMAGE_LENGTH)).save(out_file)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
