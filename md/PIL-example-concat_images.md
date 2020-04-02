---
title: PIL example concat images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'concat images'

Functions in program: 
* `def main():`
* `def concat_images(mode: str, image_paths: list):`
* `def pad_image(image: Image, desired_size: tuple, pad_value: str='#fff') -> Image:`

Modules used in program: 
* `import numpy as np`
* `import argparse`

## python concat images

Python PIL example: concat images

```python
import argparse
import numpy as np
from PIL import Image


def pad_image(image: Image, desired_size: tuple, pad_value: str='#fff') -> Image:
    """
    pad image to make image in center.

    Args:
        image: PIL.Image:
        desired_size: tuple(int): (desired width, desired height)
        pad_value: int: pad color value, default #fff is white.

    Return:
        PIL.Image: padded image.
    """
    new_im = Image.new("RGB", desired_size, color=pad_value)
    # offset make sure the image always in vertical or horizontal center.
    offset = (np.asarray(desired_size) - np.asarray(image.size)) // 2
    new_im.paste(image, box=tuple(offset))  # 2-tuple box indicate upper left corner.

    return new_im


def concat_images(mode: str, image_paths: list):
    """
    concatenate images in vertical or horizontal.

    Args:
        mode: str: mode of concatenate, value: "v": vertical; "h": horizontal;
        image_paths: list(str): image file paths.

    Return:
        None

    Raises:
        ValueError: mode value error.
    """
    # list(PIL.Image)
    images = [Image.open(f) for f in image_paths]
    # => list(zip(*(img.size for img in images)))
    # => list(zip(*((w_1, h_1), (w_2, h_2), ..., (w_n, h_n))))
    # => list(zip((w_1, h_1), (w_2, h_2), ..., (w_n, h_n)))
    # => [(w_1, w_2, ..., w_n), (h_1, h_2, ..., h_n)]
    widths, heights = list(zip(*(img.size for img in images)))

    for img in images:
        print('{0} size: {1} * {2} px'.format(img.filename, img.size[0], img.size[1]))

    if mode == 'v':
        # vertical mode, img_1 is in the above of img_2
        # the output width is max of all images' width
        # the output height is sum of all images' height
        output_width = max(widths)
        output_height = sum(heights)
        print('output size: {0} * {1} px'.format(output_width, output_height))

        # 'resize' or pad image to size (output_width, original_height)
        ary = np.vstack([np.asarray(pad_image(img, (output_width, img.size[1]))) for img in images])

    elif mode == 'h':
        # horizontal mode, img_1 is in the left of img_2
        # the output width is sum of all images' width
        # the output height is max of all images' height
        output_width = sum(widths)
        output_height = max(heights)
        print('output size: {0} * {1} px'.format(output_width, output_height))

        # 'resize' or pad image to size (original_width, output_height)
        ary = np.hstack([np.asarray(pad_image(img, (img.size[0], output_height))) for img in images])

    else:
        raise ValueError(r'-m {v, h}, the mode must be h or v.')

    output_image = Image.fromarray(ary, mode='RGB')
    output_image.save('o.jpg')


def main():
    parser = argparse.ArgumentParser(description='concatenate images.')
    # first two arguments are names of args
    # choices, the permitted values
    parser.add_argument('-m', '--mode', nargs=1, choices=['v', 'h'],
                        help='mode of concatenate type. (value: "v": vertical; "h": horizontal;).')
    # nargs, + present 1 or more args
    parser.add_argument('-f', '--files', nargs='+', help='images to be jointed.')
    # action='store_true', if set -v, value is ture
    parser.add_argument('-v', '--verbose', action='store_true', help='increase output verbosity.')

    args = parser.parse_args()
    # the type of arg value are list
    mode = args.mode[0]
    files = args.files

    if args.verbose:
        print(args)

    concat_images(mode, files)


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
