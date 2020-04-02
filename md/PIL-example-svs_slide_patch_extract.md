---
title: PIL example svs slide patch extract (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'svs slide patch extract'

Functions in program: 
* `def test(slide_path, magnification=4.0, mark_irrelevant=False):`
* `def extract_tile(`
* `def extract_tile_from_read_region(slide, patch_len, level, x_patch, y_patch):`
* `def extract_tile_from_thumbnail(thumbnail, num_horizontal_patches, num_vertical_patches, x_patch, y_patch):`
* `def get_thumbnail(slide, patch_len, level, num_horizontal_patches, num_vertical_patches):`
* `def _get_patch_len_and_level(slide, magnification):`
* `def is_patch_nonwhite(patch):`
* `def _get_grid_size(slide, patch_len, level):`

Modules used in program: 
* `import random`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`
* `import openslide`

## python svs slide patch extract

Python PIL example: svs slide patch extract

```python
import openslide
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
from PIL import Image
import random
from tqdm import tqdm


def _get_grid_size(slide, patch_len, level):
    """Get the number of horizontal and vertical patches in a grid.
       This assumes a particular patch length at a particular (SVS) level."""
    slide_width_max_mag, slide_height_max_mag = slide.level_dimensions[level]
    num_horizontal_patches = slide_width_max_mag // patch_len
    num_vertical_patches = slide_height_max_mag // patch_len
    return num_horizontal_patches, num_vertical_patches

def is_patch_nonwhite(patch):
    """Determines if a patch is white based on the saturation level"""

    # TODO: make definable
    PERCENT_WHITE_PIXELS_THRESHOLD = 0.95
    SAT_THRESHOLD = 0.05

    hsv_patch = matplotlib.colors.rgb_to_hsv(patch)
    saturation = hsv_patch[:,:,1]
    percent = np.mean(saturation < SAT_THRESHOLD)

    return percent <= PERCENT_WHITE_PIXELS_THRESHOLD

def _get_patch_len_and_level(slide, magnification):
    objective_power = int(slide.properties[openslide.PROPERTY_NAME_OBJECTIVE_POWER])
    # hasn't been tested on other objective powers...might work?
    assert(objective_power == 40)

    downsample_factor = objective_power / magnification
    level = slide.get_best_level_for_downsample(downsample_factor)
    downsample_factor = downsample_factor / slide.level_downsamples[level]

    patch_len = int(slide.properties[f'openslide.level[{level}].tile-height'])
    patch_len = int(patch_len * downsample_factor)

    return patch_len, level

def get_thumbnail(slide, patch_len, level, num_horizontal_patches, num_vertical_patches):
    horizontal_ratio = slide.level_dimensions[slide.level_count - 1][0] / slide.level_dimensions[level][0]
    vertical_ratio = slide.level_dimensions[slide.level_count - 1][0] / slide.level_dimensions[level][0]
    patch = slide.read_region(
                location=(0, 0),
                level=slide.level_count-1,
                size=(int(horizontal_ratio * num_horizontal_patches * patch_len),
                      int(vertical_ratio * num_vertical_patches * patch_len)))
    patch = np.asarray(patch, dtype=np.uint8)
    patch = patch[:, :, :3]
    # TODO: Figure out whether need following constraints.
    # assert(patch.shape[0] % num_vertical_patches == 0)
    # assert(patch.shape[1] % num_horizontal_patches == 0)
    return patch

def extract_tile_from_thumbnail(thumbnail, num_horizontal_patches, num_vertical_patches, x_patch, y_patch):
    thumbnail_width = int(thumbnail.shape[1] / num_horizontal_patches)
    thumbnail_height = int(thumbnail.shape[0] / num_vertical_patches)
    assert(thumbnail_height == thumbnail_width)
    
    thumbnail_length = thumbnail_width = thumbnail_height
    x = x_patch * thumbnail_length
    y = y_patch * thumbnail_length
    
    patch = thumbnail[y: y + thumbnail_length,
                      x: x + thumbnail_length, :]
    
    return patch

def extract_tile_from_read_region(slide, patch_len, level, x_patch, y_patch):
    level_downsample = slide.level_downsamples[level]
    x = int(x_patch * patch_len * level_downsample)
    y = int(y_patch * patch_len * level_downsample)
    patch = slide.read_region(location=(x, y),
                                      level=level,
                                      size=(patch_len, patch_len))
    patch = np.asarray(patch, dtype=np.uint8)
    patch = patch[:, :, :3]
    return patch

def extract_tile(
            slide,
            thumbnail,
            level,
            x_patch,
            y_patch,
            patch_len,
            num_horizontal_patches,
            num_vertical_patches,
            is_patch_relevant_fn=None):
    
    is_relevant = True
    
    patch = extract_tile_from_thumbnail(thumbnail,
                                        num_horizontal_patches,
                                        num_vertical_patches,
                                        x_patch,
                                        y_patch)

    if is_patch_relevant_fn is not None:
        is_relevant = is_patch_relevant_fn(patch)

    if is_relevant:
        patch =  extract_tile_from_read_region(slide,
                                               patch_len,
                                               level,
                                               x_patch,
                                               y_patch)
        is_relevant = is_patch_relevant_fn(patch)

    return patch, is_relevant
  

class PatchExtractor():
    def __init__(self, slide_path, magnification):
        """
        Args:
            slide_path (str): path to the slide
            magnification (float): magnificiation level (e.g. 20, 40)
        """
        self.slide = openslide.open_slide(slide_path)
        self.patch_len, self.level = _get_patch_len_and_level(self.slide, magnification)
        self.num_horizontal_patches, self.num_vertical_patches = \
                _get_grid_size(self.slide, self.patch_len, self.level)
        self.thumbnail = get_thumbnail(self.slide,
                                       self.patch_len,
                                       self.level,
                                       self.num_horizontal_patches,
                                       self.num_vertical_patches)
    
    def plot_patches(self, patches, mark_irrelevant=False):
        if len(patches) > 1000:
            print("Too many patches...plotting first few...")
            patches = patches[:500]
        plt.figure(figsize=(self.num_horizontal_patches, self.num_vertical_patches))
        plt.tight_layout()
        columns = self.num_horizontal_patches
        i = 0
        for patch, is_relevant in patches:
            plt.subplot(self.num_vertical_patches, self.num_horizontal_patches, i + 1)
            if mark_irrelevant and not is_relevant:
                patch = np.zeros_like(patch)
            plt.imshow(patch)
            plt.axis('off')
            i += 1
        plt.tight_layout(pad=0.2)
        plt.show()  

    
    def get_patches(self, resize_len=224, in_random_order=False, is_patch_relevant_fn=None):
        """
        Args:
            resize_len (int): size to resize the image side to.
            in_random_order (bool): whether to get random patches from the slide
                    (sampled without replacement).
            is_patch_relevant_fn (func): a function that returns whether a patch
                    is relevant given a small thumbnail

        Yields:
            patch_info: a tuple
                pil_image: the patch (no alpha channel),
                is_relevant: whether the patch is relevant
        """
        assert(self.patch_len > resize_len)
        
        if in_random_order:
            y_order = sorted(range(0, self.num_vertical_patches), key= lambda x: random.random())
            x_order = sorted(range(0, self.num_horizontal_patches), key= lambda x: random.random())
        else:
            y_order = range(0, self.num_vertical_patches)
            x_order = range(0, self.num_horizontal_patches)

        for y_patch in y_order:
            for x_patch in x_order:
                patch, is_relevant = extract_tile(self.slide,
                                                  self.thumbnail,
                                                  self.level,
                                                  x_patch,
                                                  y_patch,
                                                  self.patch_len,
                                                  self.num_horizontal_patches,
                                                  self.num_vertical_patches,
                                                  is_patch_relevant_fn)
                pil_image = Image.fromarray(patch).resize((resize_len, resize_len), Image.NEAREST)
                yield (pil_image, is_relevant)

def test(slide_path, magnification=4.0, mark_irrelevant=False):
    PE = PatchExtractor(slide_path, magnification)
    patches = list(tqdm(PE.get_patches(resize_len=224, is_patch_relevant_fn=is_patch_nonwhite),
                        total=PE.num_horizontal_patches*PE.num_vertical_patches))
    PE.plot_patches(patches, mark_irrelevant=mark_irrelevant)

    
if __name__ == '__main__':
    slide_path = "fill_in_path_to_svs.svs"
    test(slide_path)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
