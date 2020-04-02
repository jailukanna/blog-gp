---
title: PIL example vgg transform (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'vgg transform'

Functions in program: 
* `def _image_to_np(image):`
* `def resize_image(img, size):`
* `def normalize_boxes(boxes, size):`

Modules used in program: 
* `import numpy as np`
* `import pprint`

## python vgg transform

Python PIL example: vgg transform

```python
# ------------------------------------------
# Author: Cuong D. Dao
# ------------------------------------------
"""Transform operations that are specifically
used for VGG case
"""
from __future__ import absolute_import
from __future__ import division
from __future__ import print_function

import pprint
import numpy as np


def normalize_boxes(boxes, size):
    """Normalize the boxes w.r.t size (h, w)
    """
    h, w = size
    size_arr = np.array((h, w) * 2, dtype=np.float32)
    size_arr = np.expand_dims(size_arr, 0)
    size_arr = np.expand_dims(size_arr, 0)

    norm_boxes = np.divide(boxes, size_arr)
    return norm_boxes

  
class NormalizeBoxes(object):
    def __init__(self, size):
        self.size = size

    def __call__(self, boxes):
        return normalize_boxes(boxes, self.size)


class Transformer(object):
    """
    SimpleTransformer is a simple class for preprocessing and deprocessing
    images for caffe. Default values corresponds to images loaded with PIL.
    """

    def __init__(self, mean=[128, 128, 128], center_crop=[0, 0], std=None,
                 reverse_channels=False, transpose=True, expand_dims=True):
        self.mean = np.array(mean, dtype=np.float32)
        self.std = np.array(std, dtype=np.float32)
        self.reverse_channels = reverse_channels
        self.transpose = transpose
        self.expand_dims = expand_dims
        self.center_crop = center_crop

    def set_mean(self, mean):
        """
        Set the mean to subtract for centering the data.
        """
        self.mean = mean

    def __call__(self, im):
        """
        preprocess() emulate the pre-processing occuring in the vgg16 caffe
        prototxt.
        """
        if sum(self.center_crop) > 0:
            w, h = im.size
            th, tw = self.center_crop
            x1 = int(round((w - tw) / 2.))
            y1 = int(round((h - th) / 2.))
            im = im.crop((x1, y1, x1 + tw, y1 + th))

        # PIL to numpy array
        im = np.float32(np.asarray(im))

        # if self.reverse_channels:
        #     im = im[:, :, ::-1]  # change to BGR for default PIL read

        im /= 255
        im -= self.mean
        im /= self.std

        if self.transpose:
            im = im.transpose((2, 0, 1))

        if self.expand_dims:
            im = np.expand_dims(im, 0)
        return im

    def deprocess(self, im):
        """
        inverse of preprocess()
        """
        im = im.transpose(1, 2, 0)
        im += self.mean
        im = im[:, :, ::-1]  # change to RGB

        return np.uint8(im)

      
class GroupVGGTransformImage(object):
    def __init__(self, *args, **kwargs):
        self.transform = Transformer(*args, **kwargs)

    def __call__(self, images):
        images = np.array([self.transform(image) for image in images])
        images = np.squeeze(images, axis=[1])
        images = np.transpose(images, axes=[0, 2, 3, 1])
        return images


class Compose(object):
    """A class that applies a series of
    tranforms to the object passed to it
    """
    def __init__(self, transforms):
        self.transforms = transforms
    
    def __call__(self, objs):
        for t in self.transforms:
            objs = t(objs)
        return objs

def resize_image(img, size):
    """Resize the input PIL Image to the given size.
    Args:
        img (PIL Image): Image to be resized.
        size (sequence or int): Desired output size. If size is a sequence like
            (h, w), the output size will be matched to this. If size is an int,
            the smaller edge of the image will be matched to this number maintaining
            the aspect ratio. i.e, if height > width, then image will be rescaled to
            (size * height / width, size)
    
    Returns:
        PIL Image: Resized image.
    """
    if not (isinstance(size, int) or (isinstance(size, collections.Iterable) and len(size) == 2)):
        raise TypeError('Got inappropriate size arg: {}'.format(size))
    
    if isinstance(size, int):
        w, h = img.size
        if (w <= h and w == size) or (h <= w and h == size):
            return img
        if w < h:
            ow = size
            oh = int(size * h / w)
            return img.resize((ow, oh))
        else:
            oh = size
            ow = int(size * w / h)
            return img.resize((ow, oh))
    else:
        return img.resize(size[::-1])


class ResizeImage(object):
    """Resize the input PIL Image to the given size.
    Args:
        size (sequence or int): Desired output size. If size is a sequnce like
            (h, w), the output size will be matched to this. If size is an int,
            the smaller edge of the image will be matched to this number maintaining
            the aspect ratio. i.e, if height > width, then image will be rescaled to
            (size * height / width, size)
    """

    def __init__(self, size):
        assert isinstance(size, int) or (isinstance(size, collections.Iterable) and len(size) == 2)
        self.size = size
    
    def __call__(self, img):
        return resize_image(img, self.size)


def _image_to_np(image):
    """Convert a PIL Image to numpy array.
    If the original image has the 4th dimension specifying the transperancy value,
    that dimension will be remove, resulting in a correct RGB image without
    transperancy
    """
    np_image = np.array(image)
    last_dim = np_image.shape[-1]

    if last_dim == 4:
        np_image = np_image[...,:3]
    return np_image


class GroupResizeImage(object):

    def __init__(self, size):
        self.worker = ResizeImage(size)
    
    def __call__(self, img_group):
        img_group = [self.worker(img) for img in img_group]
        # Transform each image into a numpy array        
        img_group = [_image_to_np(img) for img in img_group]
        return np.array(img_group)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
