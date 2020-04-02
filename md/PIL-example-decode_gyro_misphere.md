---
title: PIL example decode gyro misphere (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'decode gyro misphere'

Functions in program: 
* `def decode_angles(R):`
* `def decode_gyro_from_file(file_path):`
* `def decode_gyro_string(data_string):`
* `def rotationMatToEuler(R):`

Modules used in program: 
* `import PIL.ExifTags`
* `import numpy as np`
* `import struct`
* `import argparse`

## python decode gyro misphere

Python PIL example: decode gyro misphere

```python
import argparse
import struct
import numpy as np

import PIL.ExifTags
from PIL import Image

ap = argparse.ArgumentParser()
group = ap.add_mutually_exclusive_group(required=True)
group.add_argument("-i", "--image", help="path to the image")
group.add_argument("-s", "--string", help="hex string from 1026 msg_id")
args = ap.parse_args()


def rotationMatToEuler(R):
    sy = np.linalg.norm(R[0:2, 0])
    singular = sy < 1e-6

    if not singular:
        x = np.arctan2(R[2, 1], R[2, 2])
        y = np.arctan2(-R[2, 0], sy)
        z = np.arctan2(R[1, 0], R[0, 0])
    else:
        x = np.arctan2(-R[1, 2], R[1, 1])
        y = np.arctan2(-R[2, 0], sy)
        z = 0

    return np.array([x, y, z])


def decode_gyro_string(data_string):
    raw_data = bytearray.fromhex(data_string)
    R = np.array([e[0] for e in struct.iter_unpack('f', raw_data)]).reshape(-1, 3)
    return R


def decode_gyro_from_file(file_path):
    im = Image.open(file_path)
    exif_data = im._getexif()
    for intval, strval in PIL.ExifTags.TAGS.items():
        if strval == 'UserComment':
            # intval will be 37510
            USERCOMMENT = intval
    raw_data = exif_data[USERCOMMENT]
    R = np.array([e[0] for e in struct.iter_unpack('f', raw_data)]).reshape(-1,  3)
    return R


def decode_angles(R):
    angles = rotationMatToEuler(R)
    angles *= 180 / np.pi
    angles *= -1
    return angles


if args.image:
    print("processing image..")
    R = decode_gyro_from_file(args.image)
elif args.string:
    print("processing string..")
    R = decode_gyro_string(args.string)

print("rotation matrix:")
print(R)
print("pitch, roll, heading(yaw):", decode_angles(R))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
