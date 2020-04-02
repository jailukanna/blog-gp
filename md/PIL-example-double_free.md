---
title: PIL example double free (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'double free'

Functions in program: 
* `def image_check(image_binary):`
* `def get_image(url, logger):`

Modules used in program: 
* `import logging`
* `import requests`

## python double free

Python PIL example: double free

```python
"""The following code run with
Wand 0.5.7
ImageMagick 6.9.7-4 Q16 x86_64 20170114 http://www.imagemagick.org
will sometimes complete, sometimes seg fault and sometimes die with a double free or corruption for the image with tag
"""
from io import BytesIO

import requests
import logging
from PIL import Image as PillowImage

size = 384, 384
new_width = 384

from wand.image import Image
from wand.color import Color
from wand.api import library
from wand.exceptions import CoderError


def get_image(url, logger):
    """
    Get Images
    """
    try:
        headers = {
            'User-Agent':
                'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36'}  # noqa
        response = requests.get(url, headers=headers)
        return response
    except Exception as error:
        logger.error("download_image_to_s3/ {} Not downloaded due to {}".format(
            url, error))
        return None

      
def image_check(image_binary):
    with Image() as img:
        pil_image = PillowImage.open(BytesIO(image_binary))
        if pil_image.format == 'TIFF':
            pil_image.verify()
            layer_level = 37724
            if layer_level in pil_image.tag:
                print("Tag present")
            else:
                print("Tag not present")
        img.read(blob=image_binary, resolution=200)


if __name__ == '__main__':
    urls = ['http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3445453/bin/pone.0045331.s002.tif', 'http://www.ncbi.nlm.nih.gov/pmc/articles/PMC1175842/bin/1471-2105-6-125-S3.tiff']
    for url in urls:
        logger = logging.getLogger()
        tr = get_image(url, logger)
        image_binary = tr.content
        a = image_check(image_binary)
        print("Wand successful:", a == None)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
