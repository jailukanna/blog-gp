---
title: PIL example S3 images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'S3 images'

Functions in program: 
* `def s3_to_local(bucket_name, prefix, target_size):`
* `def load_img(filestream, grayscale=False, color_mode='rgb', target_size=None,`

Modules used in program: 
* `import numpy as np`
* `import warnings, boto3`

## python S3 images

Python PIL example: S3 images

```python
import warnings, boto3
import numpy as np

try:
    from PIL import ImageEnhance
    from PIL import Image as pil_image
except ImportError:
    pil_image = None
    ImageEnhance = None


if pil_image is not None:
    _PIL_INTERPOLATION_METHODS = {
        'nearest': pil_image.NEAREST,
        'bilinear': pil_image.BILINEAR,
        'bicubic': pil_image.BICUBIC,
    }
    # These methods were only introduced in version 3.4.0 (2016).
    if hasattr(pil_image, 'HAMMING'):
        _PIL_INTERPOLATION_METHODS['hamming'] = pil_image.HAMMING
    if hasattr(pil_image, 'BOX'):
        _PIL_INTERPOLATION_METHODS['box'] = pil_image.BOX
    # This method is new in version 1.1.3 (2013).
    if hasattr(pil_image, 'LANCZOS'):
        _PIL_INTERPOLATION_METHODS['lanczos'] = pil_image.LANCZOS

def load_img(filestream, grayscale=False, color_mode='rgb', target_size=None,
             interpolation='nearest'):
    if grayscale is True:
        warnings.warn('grayscale is deprecated. Please use '
                      'color_mode = "grayscale"')
        color_mode = 'grayscale'
    if pil_image is None:
        raise ImportError('Could not import PIL.Image. '
                          'The use of `load_img` requires PIL.')

    img = pil_image.open(filestream)
    if color_mode == 'grayscale':
        if img.mode not in ('L', 'I;16', 'I'):
            img = img.convert('L')
    elif color_mode == 'rgba':
        if img.mode != 'RGBA':
            img = img.convert('RGBA')
    elif color_mode == 'rgb':
        if img.mode != 'RGB':
            img = img.convert('RGB')
    else:
        raise ValueError('color_mode must be "grayscale", "rgb", or "rgba"')
    if target_size is not None:
        width_height_tuple = (target_size[1], target_size[0])
        if img.size != width_height_tuple:
            if interpolation not in _PIL_INTERPOLATION_METHODS:
                raise ValueError(
                    'Invalid interpolation method {} specified. Supported '
                    'methods are {}'.format(
                        interpolation,
                        ", ".join(_PIL_INTERPOLATION_METHODS.keys())))
            resample = _PIL_INTERPOLATION_METHODS[interpolation]
            img = img.resize(width_height_tuple, resample)
    return img

s3 = boto3.resource('s3')

def s3_to_local(bucket_name, prefix, target_size):
    bucket = s3.Bucket(bucket_name)
    print(bucket.objects.filter(Prefix=prefix))
    img_list = []
    for obj in bucket.objects.filter(Prefix=prefix):
        file_name = obj.key
        response = obj.get()
        file_stream = response['Body']
        im = load_img(file_stream, target_size=target_size)
        img_list.append(np.asarray(im))

    return np.array(img_list)

#x = Image.open('HT_news.jpg')
#y = Image.open(x)
#load_img(x,target_size=(50,50))

#from test2 import s3_to_local
#s3_to_local('object-detection-dsa', 'images', (50, 50))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
