---
title: PIL example labelme2img (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'labelme2img'

Functions in program: 
* `def main():`

Modules used in program: 
* `import labelme`
* `import numpy as np`
* `import os.path as osp`
* `import os`
* `import json`
* `import glob`
* `import argparse`

## python labelme2img

Python PIL example: labelme2img

```python
#!/usr/bin/env python

from __future__ import print_function

import argparse
import glob
import json
import os
import os.path as osp

import numpy as np
from PIL import Image, ImageDraw
from base64 import b64decode

import labelme


def main():
    parser = argparse.ArgumentParser(
        formatter_class=argparse.ArgumentDefaultsHelpFormatter)
    parser.add_argument('labels_file')
    parser.add_argument('in_dir')
    parser.add_argument('out_dir')
    args = parser.parse_args()

    if osp.exists(args.out_dir):
        print('Output directory already exists:', args.out_dir)
        quit(1)
    os.makedirs(args.out_dir)

    class_names = []
    class_name_to_id = {}
    for i, line in enumerate(open(args.labels_file, encoding='utf-8').readlines()):
        class_id = i - 1  # starts with -1
        class_name = line.strip()
        class_name_to_id[class_name] = class_id
        if class_id == -1:
            assert class_name == '__ignore__'
            continue
        elif class_id == 0:
            assert class_name == '_background_'
        class_names.append(class_name)
    class_names = tuple(class_names)
    print('class_names:', class_names)

    colormap = labelme.utils.label_colormap(255)
    colormap = (colormap * 255).astype(np.uint8)

    for label_file in glob.glob(osp.join(args.in_dir, '*.json')):
        print('Generating dataset from:', label_file)
        with open(label_file) as f:
            base = osp.splitext(osp.basename(label_file))[0]
            out_img_file = osp.join(args.out_dir, base + '.jpg')

            data = json.load(f)

            imageData = data['imageData']
            img = labelme.utils.img_b64_to_arr(imageData)

            pil_img = Image.fromarray(img)
            draw = ImageDraw.Draw(pil_img)
            for shape in data['shapes']:
                point = shape['points']
                id = class_name_to_id[shape['label']]
                draw.rectangle((point[0][0], point[0][1], point[1][0], point[1][1]), outline=tuple(colormap[id].tolist()), width=10)

            pil_img.save(out_img_file)


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
