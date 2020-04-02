---
title: pil example smart-color-inverter (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'smart-color-inverter'

Functions in program: 
* `def invert_bright_connected_components(image, boundary_thickness=2, component_pct=0.02):`
* `def get_top_stat_indexes(image, stats, threshold=0.05):`
* `def connected_components_with_black_background(image, bw_threshold=255 / 2):`
* `def simple_invert(image):`

Modules used in program: 
* `import cv2`
* `import numpy as np`
* `import PIL.ImageOps`

## python smart-color-inverter

Python pil example: smart-color-inverter

```python
import PIL.ImageOps
import numpy as np
import cv2
from PIL import Image
from PIL.ImageOps import invert
from scipy.misc import imread
from matplotlib import pyplot as plt
from scipy.misc import imsave


def simple_invert(image):
    image = np.array(image)
    return 1 - image[:, :, :3] / 255.

def connected_components_with_black_background(image, bw_threshold=255 / 2):
    bw_image = np.array(image.convert('L'))
    to_invert = (bw_image > bw_threshold).astype(np.uint8)
    num_labels, labels, stats, centroids = cv2.connectedComponentsWithStats(
        to_invert, connectivity=4)
    if labels[np.where(labels == 0)].mean() == 1:
        to_invert = (bw_image < bw_threshold).astype(np.uint8)
        num_labels, labels, stats, centroids = cv2.connectedComponentsWithStats(
            to_invert, connectivity=4)
    return num_labels, labels, stats, centroids

def get_top_stat_indexes(image, stats, threshold=0.05):
    top_area_indexes = np.argsort(stats[:, 4])[::-1]
    top_areas = stats[top_area_indexes, 4]
    top_area_percents = top_areas / np.prod(image.size)
    area_indexes_above_threshold = top_area_indexes[:np.argmin(top_area_percents > threshold)]
    return area_indexes_above_threshold


def invert_bright_connected_components(image, boundary_thickness=2, component_pct=0.02):
    np_image = np.array(image)[:, :, :3]
    num_labels, labels, stats, centroids = connected_components_with_black_background(image)
    top_stat_indexes = get_top_stat_indexes(image, stats, component_pct)

    for idx in range(top_stat_indexes.shape[0]):
        if top_stat_indexes[idx] == 0:
            # ignore background
            continue
        search_labels = labels == top_stat_indexes[idx]

        _, contours, _ = cv2.findContours(
            search_labels.astype(np.uint8) * 255,
            cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
        search_labels = search_labels.astype(np.float)

        cv2.fillPoly(search_labels, [contours[0]], 1)
        np_image[np.where(search_labels)] = 255 - np_image[np.where(search_labels)]
        
        if boundary_thickness > 0:
            _, full_contours, _ = cv2.findContours(
                search_labels.astype(np.uint8) * 255,
                cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
            contour_im = cv2.drawContours(search_labels, full_contours[0], -1, 0.5, boundary_thickness)
            np_image[np.where(contour_im == 0.5)] = 255

    return np_image

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
