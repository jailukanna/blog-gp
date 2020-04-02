---
title: pil example holoviews video export (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'holoviews video export'


Modules used in program: 
* `import imageio`
* `import time`
* `import os`
* `import gzip`
* `import io`
* `import re`
* `import numpy as np`
* `import holoviews as hv`
* `import PIL.Image`

## python holoviews video export

Python pil example: holoviews video export

```python
import PIL.Image
import holoviews as hv
import numpy as np
import re
import io
import gzip
import os
import time
from holoviews.plotting.mpl import MPLRenderer
import imageio

ef make_video_from_hmap(hmap, name, fps=2):
    renderer = hv.renderer('matplotlib')
    writer = imageio.get_writer(name, fps=fps)
    size = None

    if isinstance(hmap, hv.Layout) or isinstance(hmap, hv.NdLayout):
        keys = hmap[hmap.keys()[0]].keys()
        for key in keys:
            print("writing key {}".format(key))
            dim = hmap.dimensions()[1]
            label = dim.name + ": " + dim.pprint_value(key)
            if dim.unit != None:
                label += " " + dim.unit
            item = hmap[:, key]
            item = item.relabel(label)
            hv.renderer('matplotlib').save(item, '_temp', fmt='png')
            img = PIL.Image.open("_temp.png")
            arr = np.array(img)
            if size == None:
                size = arr.shape[0:2]
            else:
                arr = resize_frame(arr, size)
            writer.append_data(arr)

    else:
        for key, item in hmap.items():
            print("writing key {}".format(key))
            label = ""
            if not isinstance(key, tuple):
                key = tuple([key])
            for value, dim in zip(key, hmap.dimensions()):
                dim_label = dim.name + ": " + dim.pprint_value(value)
                if dim.unit != None:
                    dim_label += " " + dim.unit
                if label == "":
                    label = dim_label
                else:
                    label += ", " + dim_label
            item = item.relabel(label)
            hv.renderer('matplotlib').save(item, '_temp', fmt='png')
            img = PIL.Image.open("_temp.png")
            arr = np.array(img)
            if size == None:
                size = arr.shape[0:2]
            else:
                arr = resize_frame(arr, size)
            writer.append_data(arr)
    writer.close()
    os.remove("_temp.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
