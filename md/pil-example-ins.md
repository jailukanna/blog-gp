---
title: pil example ins (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ins'

Functions in program: 
* `def main():`
* `def get_class_map():`

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

## python ins

Python pil example: ins

```python
#!/usr/bin/env python

from __future__ import print_function

import argparse
import glob
import json
import os
import os.path as osp
import sys
from pprint(import pprint)
from tqdm import tqdm

import numpy as np
import PIL.Image

import labelme

def get_class_map():
    return {
        '_background_': 0,
        'Apple_porcelain': 0,
        'Beer': 0,
        'Book': 0,
        'Carbonated_drinks': 0,
        'Cup': 0,
        'Dispenser': 0,
        'Desk_lamp': 0,
        'Drugs': 0,
        'Glasses_case': 0,
        'Kettle': 0,
        'Milk': 0,
        'Mug': 0,
        'Metal_box': 0,
        'Nipper': 0,
        'Plate': 0,
        'Potato_chips': 0,
        'Seasoning_sauce': 0,
        'Spoon': 0,
        'Stick_snacks': 0,
        'Toy': 0,
        'Wash_product': 0,
    }
    
def main():
    parser = argparse.ArgumentParser(
        formatter_class=argparse.ArgumentDefaultsHelpFormatter
    )
    parser.add_argument('--input', help='input annotated directory')
    parser.add_argument('--output', help='output dataset directory')
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
            
            shsh = data['shapes']
            class_map = get_class_map()
            
            for idx, sh in enumerate(shsh):
                label = sh['label']
                data['shapes'][idx]['label'] = label + str(class_map[label])
                class_map[label] += 1

            llk = [ds['label'] for ds in data['shapes']]
            llk.append('_background_')
            
            class_id = dict()
            
            for idx, ll in enumerate(llk):
                class_id[ll] = idx
            
            img_file = osp.join(osp.dirname(label_file), data['imagePath'])
            img = np.asarray(PIL.Image.open(img_file))
            PIL.Image.fromarray(img).save(out_img_file)

            lbl = labelme.utils.shapes_to_label(
                img_shape=img.shape,
                shapes=data['shapes'],
                label_name_to_value=class_id,
            )
            labelme.utils.lblsave(out_png_file, lbl)

            np.save(out_lbl_file, lbl)

            viz = labelme.utils.draw_label(
                lbl, img, tuple(llk), colormap=colormap)
            PIL.Image.fromarray(viz).save(out_viz_file)


if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
