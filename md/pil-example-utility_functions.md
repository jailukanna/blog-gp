---
title: pil example utility functions (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'utility functions'

Functions in program: 
* `def default_loader_unaligned(path):`
* `def make_dataset(dir):`
* `def is_image_file(filename):`
* `def image_load_fkt_pillow_aligned_label_x(filename, desired_size, n_channels=1):`
* `def image_load_fkt_pillow_unaligned(filename, desired_size, n_channels=1):`

Modules used in program: 
* `import torch`
* `import os.path`
* `import os`
* `import torch.utils.data as data`
* `import random`
* `import numpy as np`

## python utility functions

Python pil example: utility functions

```python
import numpy as np
import random
import torch.utils.data as data
from PIL import Image
import os
import os.path
from torchvision import transforms
import torch


def image_load_fkt_pillow_unaligned(filename, desired_size, n_channels=1):
    """
    Function to load and resize images with one or 3 channels
    :param filename: name of the file the image should be loaded from
    :param desired_size: size and width the image should be resized to (currently only same width and height are supported)
    :param n_channels: number of color channels
    :return: image as numpy array
    """
    img = Image.open(filename).convert('RGB')
    if n_channels == 1:
        img = img.convert('L')
    img = img.resize(size=(desired_size, desired_size), resample=Image.BILINEAR)
    img = np.array(img)
    img = np.reshape(img, (img.shape[0], img.shape[1], n_channels))
    return img


def image_load_fkt_pillow_aligned_label_x(filename, desired_size, n_channels=1):
    img = Image.open(filename).convert('RGB')
    if n_channels == 1:
        img = img.convert('L')
    img = img.resize(size=(desired_size, desired_size), resample=Image.BILINEAR)
    img = np.array(img)
    img = np.reshape(img, (img.shape[0], img.shape[1], n_channels))

    label_file = str(filename).rsplit(".", maxsplit=1)[1] + ".txt"
    with open(label_file, "r") as f:
        label_lines = f.readlines()

    for line in label_lines:
        label_x = int(line.split(",")[0])
        label_y = int(line.split(",")[1])

    if str(filename).rsplit(".", maxsplit=1)[1].endswith("a"):
        endstring = "b"
    else:
        endstring = "a"

    opposite_data_set = [x for x in os.listdir(os.path.split(filename)[0]) if x.endswith(endstring)]
    possible_files = []
    for file in opposite_data_set:
        file_path = os.path.join(os.path.split(filename)[0], file)
        with open(str(file_path.rsplit(".", maxsplit=1)[1]) + ".txt", "r") as f:
            opposite_label_lines = f.readlines()

        for line in opposite_label_lines:
            opposite_label_x = int(line.split(",")[0])
            opposite_label_y = int(line.split(",")[1])

        if -5 <= (label_x - opposite_label_x) <= 5:
            possible_files.append(file_path)

    aligned_img = Image.open(random.choice(possible_files))
    if n_channels == 1:
        aligned_img = aligned_img.convert('L')
    aligned_img = aligned_img.resize(size=(desired_size, desired_size), resample=Image.BILINEAR)
    aligned_img = np.array(aligned_img)
    aligned_img = np.reshape(aligned_img, (aligned_img.shape[0], aligned_img.shape[1], n_channels))

    return [img, aligned_img]

IMG_EXTENSIONS = [
    '.jpg', '.JPG', '.jpeg', '.JPEG',
    '.png', '.PNG', '.ppm', '.PPM', '.bmp', '.BMP',
]


def is_image_file(filename):
    """
    Helper Function to determine whether a file is an image file or not
    :param filename: the filename containing a possible image
    :return: True if file is image file, False otherwise
    """
    return any(filename.endswith(extension) for extension in IMG_EXTENSIONS)


def make_dataset(dir):
    """
    Helper Function to make a dataset containing all images in a certain directory
    :param dir: the directory containing the dataset
    :return: images: list of image paths
    """
    images = []
    assert os.path.isdir(dir), '%s is not a valid directory' % dir

    for root, _, fnames in sorted(os.walk(dir)):
        for fname in fnames:
            if is_image_file(fname):
                path = os.path.join(root, fname)
                images.append(path)

    return images


def default_loader_unaligned(path):
    """
    Helper function to load an Image with PIL
    :param path: path of image file
    :return: loaded image in RGB mode as PIL Image
    """
    return Image.open(path).convert('RGB')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
