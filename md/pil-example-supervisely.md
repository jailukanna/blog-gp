---
title: pil example supervisely (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'supervisely'

Functions in program: 
* `def mask_2_base64(mask):`
* `def base64_2_mask(s):`

Modules used in program: 
* `import json`
* `import cv2`
* `import numpy as np`
* `import zlib`
* `import base64`
* `import os`

## python supervisely

Python pil example: supervisely

```python
#!/usr/bin/python
# -*- coding=utf8 -*-
"""
# @Author : lart
# @Created Time : 2019-01-16 19:59:51
# @Description : the converter for the datasets from supervise.ly
"""
import os
import base64
import zlib
import numpy as np
import cv2
import json


def base64_2_mask(s):
    z = zlib.decompress(base64.b64decode(s))
    n = np.fromstring(z, np.uint8)
    mask = cv2.imdecode(n, cv2.IMREAD_UNCHANGED)[:, :, 3].astype(int)
    return mask


def mask_2_base64(mask):
    img_pil = Image.fromarray(np.array(mask, dtype=np.uint8))
    img_pil.putpalette([0, 0, 0, 255, 255, 255])
    bytes_io = io.BytesIO()
    img_pil.save(bytes_io, format='PNG', transparency=0, optimize=0)
    bytes = bytes_io.getvalue()
    return base64.b64encode(zlib.compress(bytes)).decode('utf-8')

ann_path = 'ann'
png_path = 'ann_png'
img_path = 'img'
datasets_root = '/home/lart/Datasets/sypervisely_json'
# /home/lart/Datasets/sypervisely_json/Supervisely Person Dataset__ds1/ann

for folder in os.listdir(datasets_root):
    folder_path = os.path.join(datasets_root, folder)
    
    # 是文件夹
    if os.path.isdir(folder_path):
        data_files = os.path.join(folder_path, ann_path)
        save_path = os.path.join(folder_path, png_path)
        print(save_path)
        
        if not os.path.exists(save_path):
            os.makedirs(save_path)
        
        for name_file in os.listdir(data_files):
            json_path = os.path.join(data_files, name_file)
            
            with open(json_path, "r") as json_file:
                data = json.load(json_file)
                
                if data['objects'][0]['bitmap'] is not None:
                    origin = data['objects'][0]['bitmap']['origin']
                    axis_y, axis_x = origin
                    
                    size_full = [data['size']['height'], data['size']['width']]
                    height, width = size_full
                    data = data['objects'][0]['bitmap']['data']
                    
                    img_full = np.zeros(size_full)
                    mask = base64_2_mask(data)
                    img_full[
                        axis_x:axis_x + mask.shape[0], axis_y:axis_y + mask.shape[1]
                    ] = mask
                    file_out_path = os.path.join(save_path, name_file)
                    cv2.imwrite(file_out_path[:-4]+'.png', img_full)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
