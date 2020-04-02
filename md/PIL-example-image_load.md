---
title: PIL example image load (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image load'

Functions in program: 
* `def convert_pil_cv2(pil_image):`
* `def using_pil_shrink(f):`
* `def using_vips(f):`
* `def using_cv2(f):`
* `def using_pil(f):`

Modules used in program: 
* `import cProfile`
* `import time`
* `import numpy as np`
* `import timeit`
* `import cv2`
* `import pyvips`

## python image load

Python PIL example: image load

```python
#!/usr/bin/env python3

import pyvips
import cv2
from PIL import Image

import timeit
import numpy as np
import time
import cProfile

def using_pil(f):
    im = Image.open(f)
    return np.asarray(im)

def using_cv2(f):
    arr = cv2.imread(f, cv2.IMREAD_UNCHANGED)
    return arr

def using_vips(f):
    image = pyvips.Image.new_from_file(f, access='sequential')
    mem_img = image.write_to_memory()
    imgnp=np.frombuffer(mem_img, dtype=np.uint8).reshape(image.height, image.width, 3)
    return imgnp

def using_pil_shrink(f):
    img = Image.open(f).draft('RGB', (400, 400)).convert('RGB')
    # if img.format == 'JPEG' or img.format == 'MPO':

    # img.draft('RGB', (400, 400))

    img = img.resize((400, 400))
    np_img = np.array(img)
    print(np_img.shape)
    return img

def convert_pil_cv2(pil_image):
    # print(pil_image.size)
    cv2_image = cv2.cvtColor(np.array(pil_image), cv2.COLOR_RGB2GRAY)
    _ = np.array(pil_image)
    return cv2_image


image_file = '/home/milab/Downloads/large_mo.jpg'
# image_file = '/home/milab/Downloads/test.jpg'
# image_file = '/home/milab/Downloads/test_400.jpg'
# image_file = '/home/milab/Downloads/png_file.png'

pil_image = Image.open(image_file)

# pil_image.resize()
print(pil_image.format)
print(pil_image.size)


# cProfile.run('usingPIL("{}")'.format(image_file))
# cProfile.run('usingOpenCV("{}")'.format(image_file))
# cProfile.run('usingVIPS("{}")'.format(image_file))
# cProfile.run('using_pil_shrink("{}")'.format(image_file))

# print('using_pil: \t\t\t{:.1f}ms'.format(timeit.timeit('using_pil("{}")'.format(image_file), number=10, setup="from __main__ import using_pil")/10 * 1000))
# print('using_cv2: \t\t\t{:.1f}ms'.format(timeit.timeit('using_cv2("{}")'.format(image_file), number=10, setup="from __main__ import using_cv2")/10 * 1000))
#
# start_time = time.time()
# for i in range(10):
#     _ = convert_pil_cv2(pil_image)
# end_time = (time.time() - start_time) / 10
# print('convert_pil_cv2: \t{:.1f}ms'.format(end_time * 1000))
#
print('using_pil_shrink: \t{:.1f}ms'.format(timeit.timeit('using_pil_shrink("{}")'.format(image_file), number=10, setup="from __main__ import using_pil_shrink")/10 * 1000))







```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
