---
title: PIL example seman (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'seman'

Functions in program: 
* `def main():`

Modules used in program: 
* `import labelme`
* `import PIL.Image`
* `import numpy as np`
* `import sys`
* `import os.path as osp`
* `import os`
* `import json`
* `import glob`
* `import argparse`

## python seman

Python PIL example: seman

```python
#!/usr/bin/env python

from __future__ import print_function

import argparse
import glob
import json
import os
import os.path as osp
import sys
from tqdm import tqdm

import numpy as np
import PIL.Image

import labelme


def main():
    parser = argparse.ArgumentParser(
        formatter_class=argparse.ArgumentDefaultsHelpFormatter
    )
    parser.add_argument('--input', help='input annotated directory')
    parser.add_argument('--output', help='output dataset directory')
    parser.add_argument('--labels', help='labels file', required=True)
    args = parser.parse_args()

    if osp.exists(args.output):
        print('Output directory already exists:', args.output)
    else:
        os.makedirs(args.output)
        os.makedirs(osp.join(args.output, 'JPEGImages'))
        os.makedirs(osp.join(args.output, 'SegmentationClass'))
        os.makedirs(osp.join(args.output, 'SegmentationClassPNG'))
        os.makedirs(osp.join(args.output, 'SegmentationClassVisualization'))
    print('Creating dataset:', args.output)

    class_names = []
    class_name_to_id = {}
    for i, line in enumerate(open(args.labels).readlines()):
        class_name = line.strip()
        class_name_to_id[class_name] = i
        
        class_names.append(class_name)
        
    print(class_name_to_id)
    class_names = tuple(class_names)
    print('class_names:', class_names)
    out_class_names_file = osp.join(args.output, 'class_names.txt')
    with open(out_class_names_file, 'w') as f:
        f.writelines('\n'.join(class_names))
    print('Saved class_names:', out_class_names_file)

    colormap = labelme.utils.label_colormap(255)

    for label_file in tqdm(glob.glob(osp.join(args.input, '*.json'))):
        print('Generating dataset from:', label_file)
        with open(label_file) as f:
            base = osp.splitext(osp.basename(label_file))[0]
            out_img_file = osp.join(
                args.output, 'JPEGImages', base + '.jpg')
            out_lbl_file = osp.join(
                args.output, 'SegmentationClass', base + '.npy')
            out_png_file = osp.join(
                args.output, 'SegmentationClassPNG', base + '.png')
            out_viz_file = osp.join(
                args.output,
                'SegmentationClassVisualization',
                base + '.jpg',
            )

            data = json.load(f)

            img_file = osp.join(osp.dirname(label_file), data['imagePath'])
            img = np.asarray(PIL.Image.open(img_file))
            PIL.Image.fromarray(img).save(out_img_file)

            lbl = labelme.utils.shapes_to_label(
                img_shape=img.shape,
                shapes=data['shapes'],
                label_name_to_value=class_name_to_id,
            )
            labelme.utils.lblsave(out_png_file, lbl)

            np.save(out_lbl_file, lbl)

            viz = labelme.utils.draw_label(
                lbl, img, class_names, colormap=colormap)
            PIL.Image.fromarray(viz).save(out_viz_file)


if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
