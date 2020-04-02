---
title: pil example repeat gen (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'repeat gen'

Functions in program: 
* `def pil_transform(image_str):`

Modules used in program: 
* `import numpy as np`

## python repeat gen

Python pil example: repeat gen

```python
from PIL import Image
import numpy as np

# call with pil_transform('test.jpg')
def pil_transform(image_str):
    
    # padding process started
    im_opn = Image.open(image_str)
    old_sz = im_opn.size
    new_sz = (old_sz[0]+30, old_sz[1]+30)
    new_im = Image.new('RGB',new_sz)
    new_im.paste(im_opn, ( int((new_sz[0]-old_sz[0])/2), int((new_sz[1]-old_sz[1])/2)) )
    # padding process completed
    
    # images to be passed onto as for grid formation, for 10 x 10 grid pass 10^2 PIL object images .
    images=[new_im for i in range (0,100)]
    
    # grid creation and saving the it in jpg form
    # of size 1024 x 1024 resultant image.
    
    max_horiz=10
    n_images = len(images)
    n_horiz = min(n_images, max_horiz)
    h_sizes, v_sizes = [0] * n_horiz, [0] * (n_images // n_horiz)
    for i, im in enumerate(images):
        h, v = i % n_horiz, i // n_horiz
        h_sizes[h] = max(h_sizes[h], im.size[0])
        v_sizes[v] = max(v_sizes[v], im.size[1])
    h_sizes, v_sizes = np.cumsum([0] + h_sizes), np.cumsum([0] + v_sizes)
    im_grid = Image.new('RGB', (h_sizes[-1], v_sizes[-1]), color='white')
    for i, im in enumerate(images):
        im_grid.paste(new_im, (h_sizes[i % n_horiz], v_sizes[i // n_horiz]))
    im_grid.thumbnail((1024, 1024), Image.ANTIALIAS)
    im_grid.save(image_str,'JPEG',quality=72)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
