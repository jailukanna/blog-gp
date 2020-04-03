---
title: matplotlib example ai legends (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'ai legends'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import os`

## python ai legends

Python matplotlib example: ai legends

```python
import os
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.transforms import TransformedBbox, Bbox
from matplotlib.image import BboxImage
from matplotlib.legend_handler import HandlerBase
from matplotlib._png import read_png
from matplotlib.cbook import get_sample_data

class ImageHandler(HandlerBase):
    def create_artists(self, legend, orig_handle,
                       xdescent, ydescent, width, height, fontsize,
                       trans):

        # enlarge the image by these margins
        sx, sy = self.image_stretch 

        # create a bounding box to house the image
        bb = Bbox.from_bounds(xdescent - sx,
                              ydescent - sy,
                              width + sx,
                              height + sy)

        tbb = TransformedBbox(bb, trans)
        image = BboxImage(tbb)
        image.set_data(self.image_data)

        self.update_prop(image, orig_handle, legend)

        return [image]

    def set_image(self, image_path, image_stretch=(0, 0)):
        if not os.path.exists(image_path):
            sample = get_sample_data('/hinton.png', asfileobj=False)
            self.image_data = read_png(sample)
        else:
            self.image_data = read_png(image_path)

        self.image_stretch = image_stretch
    def set_image2(self, image_path, image_stretch=(0, 0)):
        if not os.path.exists(image_path):
            sample = get_sample_data("/yoshua.png", asfileobj=False)
            self.image_data = read_png(sample)
        else:
            self.image_data = read_png(image_path)

        self.image_stretch = image_stretch

# random data
x = np.random.randn(100)
y = np.random.randn(100)
y2 = np.random.randn(100)

# plt.figure(figsize=(20,20))
# plot two series of scatter data
plt.figure(figsize=(6,4), dpi=80)
s = plt.scatter(x, y, c='b')
s = plt.scatter(x, y2, c='r')


# setup the handler instance for the scattered data
custom_handler = ImageHandler()
custom_handler.set_image("[PATH TO IMAGE]",
                         image_stretch=(5, 30)) 


custom_handler2 = ImageHandler()
custom_handler2.set_image2("[PATH TO IMAGE]",
                         image_stretch=(5, 30)) 

# add the legend for the scattered data, mapping the
# scattered points to the custom handler

plt.legend([s, s2],
           ['Legend-1', 'Legend-2'],
           handler_map={s: custom_handler, s2: custom_handler2},
           labelspacing=2,
           frameon=False)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
