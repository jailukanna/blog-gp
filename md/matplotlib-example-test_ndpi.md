---
title: matplotlib example test ndpi (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'test ndpi'


Modules used in program: 
* `import time`
* `import matplotlib.patches as patches`
* `import matplotlib.pyplot as plt`
* `import json`
* `import cv2`
* `import pickle`
* `import numpy as np`
* `import json`
* `import ndpread`
* `import sys`

## python test ndpi

Python matplotlib example: test ndpi

```python
import sys
sys.path.append("/data/jimmy15923/CGMH_NPC_PROGRAM/ndpi2dzi/") ## set path for ndpi
import ndpread
import json
import numpy as np
import pickle
import cv2
from matplotlib.path import Path
import json
import matplotlib.pyplot as plt
from matplotlib.path import Path
import matplotlib.patches as patches
import time
%matplotlib inline

labels_path = "/data/jimmy15923/CGMH_NPC_PROGRAM/CGMH_NPC_labels.json"
with open(labels_path) as f:
    label = json.load(f)
    
npc = []
be = []
bg = []
for slide in range(259):
    for i, l in enumerate(label[slide]['labels']):
        if l['name'] == "NPC":
            npc.append([slide, i])
        elif l['name'] == 'Benign':
            be.append([slide, i])
        elif l['name'] == 'Background':
            bg.append([slide, i])
            
idx = 0
done = False
patch_side_length = 256
crop_list = []
result = []
t = time.time()
for i in npc:
    ndpi_file = label[i[0]]['ndpi_file'].replace("nas", "dataset")
    for seg in label[i[0]]['labels'][i[1]]['data']:
        segments = label[i[0]]['labels'][i[1]]['data'][seg][1]['segments']
        if len(segments) > 1:
            ploygon = Path(np.array(segments), closed=True)
            bounding_box = ploygon.get_extents()

            # generate squares
            x_list = np.arange(bounding_box.x0, bounding_box.x1, patch_side_length)
            y_list = np.arange(bounding_box.y0, bounding_box.y1, patch_side_length)
            for x0 in x_list:
                x1 = x0 + patch_side_length
                for y0 in y_list:
                    y1 = y0 + patch_side_length
                    square = Path(np.array([[x0, y0], [x0, y1], [x1, y1], [x1, y0]]))
                    if ploygon.contains_path(square):
                        crop_list.append({'x0': x0, 'y0': y0})
            for crop in crop_list:
                temp_img = ndpread.getImg(bytes(ndpi_file, 'utf-8'), crop['y0'], crop['x0'], patch_side_length, patch_side_length, 0)
                # crop_image_file = 'tmp/test_pixelise/%s_%s_%s.jpg' % (markId, crop['x0'], crop['y0'])
                # cv2.imwrite(crop_image_file, temp_img, [cv2.IMWRITE_JPEG_QUALITY, 75])
                # time.sleep(0.3)
                result.append(temp_img)
                idx += 1
                print(idx)
                if idx == 32:
                    print(time.time() - t)
                    x_train = np.array(result)
                    print(done)
                    idx = 0


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
