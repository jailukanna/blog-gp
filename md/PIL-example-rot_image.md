---
title: PIL example rot image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'rot image'

Functions in program: 
* `def create_rot_image(img_file, angle=-15, duration=40):`

Modules used in program: 
* `import argparse`
* `import re`
* `import numpy as np`
* `import cv2`

## python rot image

Python PIL example: rot image

```python
import cv2
import numpy as np
from PIL import Image

import re
import argparse

def create_rot_image(img_file, angle=-15, duration=40):
    size = (128, 128)
    center = (128//2, 128//2)
    scale = 1.0

    img = cv2.imread(img_file)
    if img is None:
        raise ValueError(f"Not found such a file {img_file}")
    print("[+] Image Loaded")
    img = cv2.resize(img, (128, 128))

    images = []
    for i in range(360//abs(angle)):
        rot_angle = angle * i
        rot_mat = cv2.getRotationMatrix2D(center, rot_angle, scale)
        img_rot = cv2.warpAffine(img, rot_mat, size, flags=cv2.INTER_CUBIC)
        img_rot = cv2.cvtColor(img_rot, cv2.COLOR_BGR2RGB)

        black = [0, 0, 0]
        white = [255, 255, 255]
        img_rot[np.where((img_rot == black).all(axis=2))] = white

        pil_img_rot = Image.fromarray(img_rot)
        images.append(pil_img_rot)

    savefile = re.sub(r".(png|jpg)$", ".gif", img_file)
    images[0].save(savefile,
                save_all=True, append_images=images[1:], optimize=False, duration=duration, loop=0)
    print("[+] Rot Image Saved")


if __name__ == "__main__":
    arg_parser = argparse.ArgumentParser()
    arg_parser.add_argument("file")
    arg_parser.add_argument("--angle", default=-15)
    arg_parser.add_argument("--duration", default=40)
    args = arg_parser.parse_args()

    file_name = args.file
    angle = int(args.angle)
    duration = int(args.duration)

    create_rot_image(file_name, angle, duration)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
