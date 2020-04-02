---
title: pil example load img (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'load img'

Functions in program: 
* `def load_img(path, grayscale=False, target_size=None):`

## python load img

Python pil example: load img

```python
def load_img(path, grayscale=False, target_size=None):
    """Loads an image into PIL format.

    # Arguments
        path: Path to image file
        grayscale: Boolean, whether to load the image as grayscale.
        target_size: Either `None` (default to original size)
            or tuple of ints `(img_height, img_width)`.

    # Returns
        A PIL Image instance.

    # Raises
        ImportError: if PIL is not available.
    """
    if pil_image is None:
        raise ImportError('Could not import PIL.Image. '
                          'The use of `array_to_img` requires PIL.')
    img = pil_image.open(path)
    if grayscale:
        img = img.convert('L')
    else:  # Ensure 3 channel even when loaded image is grayscale
        img = img.convert('RGB')
    if target_size:
        
        old_size = img.size
        new_size = (max(old_size), max(old_size))
        new_im = pil_image.new("RGB", new_size)   ## luckily, this is already black!
        new_im.paste(img, ((new_size[0]-old_size[0])//2,
                              (new_size[1]-old_size[1])//2))

        img = new_im.resize((target_size[1], target_size[0]))
    return img

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
