---
title: PIL example image manipulation (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image manipulation'

Functions in program: 
* `def resize_to_smallest_parameter(image_object, num_pixels_height, num_pixels_wide):`
* `def resize_by_height(image_object, num_pixels_height):`
* `def resize_by_width(image_object, num_pixels_wide):`

Modules used in program: 
* `import PIL`

## python image manipulation

Python PIL example: image manipulation

```python
'''
This module contains functions that are used to resize an image and maintain its aspect ratio.
It relies on the PIL library that can be installed using pip by issuing the command below

$ pip install pillow

Credit where credit is due, These functions where modified based based on the code published at this link:
https://gist.github.com/tomvon/ae288482869b495201a0

'''

import PIL
from PIL import Image


def resize_by_width(image_object, num_pixels_wide):
    '''Function that resizes an image, keeping its aspect ratio, constraining its dimensions
    by the desired output width in pixels

    :param image_object: Image opened with Pil ex. Image.open('/path/to/img.jpg')
    :param num_pixels_wide: desired width
    :type num_pixels_wide: int
    :return: image_object that has been resized
    '''
    # Calculate the needed height of image to maintain aspect ratio
    wpercent = (num_pixels_wide / float(image_object.size[0]))
    num_pixels_height = int((float(image_object.size[1]) * float(wpercent)))

    # Resize image and return to user
    return image_object.resize((num_pixels_wide, num_pixels_height), PIL.Image.ANTIALIAS)


def resize_by_height(image_object, num_pixels_height):
    '''Function that resizes an image, keeping its aspect ratio, constraining its dimensions
    by the desired output height in pixels

    :param image_object: Image opened with Pil ex. Image.open('/path/to/img.jpg')
    :param num_pixels_height: desired height
    :type num_pixels_height: int
    :return: mage_object that has been resized
    '''

    # Calculate the needed width of image to maintain aspect ratio
    hpercent = (num_pixels_height / float(image_object.size[1]))
    num_pixels_wide = int((float(image_object.size[0]) * float(hpercent)))

    # Resize image and return to user
    return image_object.resize((num_pixels_wide, num_pixels_height), PIL.Image.ANTIALIAS)


def resize_to_smallest_parameter(image_object, num_pixels_height, num_pixels_wide):
    '''This function will resize an image keeping its aspect ratio based on the most restrictive
    constraint. First it resizes the image by height, then by width, whichever image is smaller
    it returns that image

    :param image_object:
    :param num_pixels_height:
    :param num_pixels_wide:
    :return:
    '''
    img_by_height = resize_by_height(image_object, num_pixels_height)
    img_by_width = resize_by_width(image_object, num_pixels_wide)

    if img_by_height.size > img_by_width.size:
        return img_by_width
    else:
        return img_by_height


if __name__ == '__main__':

    desired_width = int(input('Enter number of pixels wide: '))
    desired_height = int(input('Enter number of pixels high: '))
    image_file = input('Enter path to image file: ')
    img = Image.open(image_file)
    print(img.size)
    img = resize_by_width(img, desired_width)
    print(img.size)
    img = resize_by_height(img, desired_height)
    print(img.size)
    img.save(input('Enter output name: '))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
