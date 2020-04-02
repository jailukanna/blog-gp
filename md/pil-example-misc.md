---
title: pil example misc (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'misc'

Functions in program: 
* `def show(img):`
* `def draw_bbox(img: Image, label: tuple):`

Modules used in program: 
* `import os`
* `import cv2`
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python misc

Python pil example: misc

```python
# How to open an image file
from PIL import Image
img = Image.open(img_path)


# How to draw a bounding box with PIL
from PIL import Image, ImageDraw

def draw_bbox(img: Image, label: tuple):
    # Starting from upper left corner
    x1, y1, x2, y2 = label
    d = ImageDraw.Draw(img)
    d.rectangle((x1, y1, x2, y2), outline=(0, 255, 0))

    

# How to visualize torch.tensor in Jupyter

import matplotlib.pyplot as plt
import numpy as np

def show(img):
    img -= torch.min(img)
    img /= torch.max(img)
    
    if len(img.shape) == 4:
        assert img.shape[0] == 1
        img = img[0]
    if len(img.shape) == 3 and img.shape[0] == 1:
        img = torch.cat((img, img, img), 0)

    npimg = img.numpy()
    plt.imshow(np.transpose(npimg, (1,2,0)), interpolation='nearest')

    
# More examples can be found here https://github.com/pytorch/vision/blob/master/test/sanity_checks.ipynb


# How to make a movie out of images in Python
# https://stackoverflow.com/questions/44947505/how-to-make-a-movie-out-of-images-in-python

import cv2
import os

image_folder = 'images'
video_name = 'video.avi'

images = [img for img in os.listdir(image_folder) if img.endswith(".png")]
frame = cv2.imread(os.path.join(image_folder, images[0]))
height, width, layers = frame.shape

video = cv2.VideoWriter(video_name, -1, 1, (width,height))

for image in images:
    video.write(cv2.imread(os.path.join(image_folder, image)))

cv2.destroyAllWindows()
video.release()

# In ubuntu you may need to replace one line
video = cv2.VideoWriter(video_name,
                        cv2.VideoWriter_fourcc(*'XVID'),
                        30,
                        (width,height))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
