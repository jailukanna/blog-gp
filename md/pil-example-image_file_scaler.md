---
title: pil example image file scaler (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'image file scaler'

Functions in program: 
* `def main():`
* `def get_image_size(pil_image):`
* `def get_scaled_image(pil_image, scale):`
* `def is_in_range(value, target, error):`
* `def process_image_size(pil_image, filesize):`
* `def image_download(origin_url, filesize):`

Modules used in program: 
* `import urllib.request`
* `import sys`
* `import datetime`

## python image file scaler

Python pil example: image file scaler

```python
import datetime
import sys
import urllib.request
from io import BytesIO
from urllib.parse import urlparse

from PIL import Image


def image_download(origin_url, filesize):
    im = Image.open(urllib.request.urlopen(origin_url))
    im = process_image_size(im, filesize)
    return im

def process_image_size(pil_image, filesize):
    image_size_limit = filesize * 1024 * 1024
    current_size = get_image_size(pil_image)

    error = 50 * 1024
    
    inferior_scale = 0.1
    superior_scale = 1
    search_scale = ((superior_scale - inferior_scale) / 2) + inferior_scale
    search_size = get_image_size(pil_image)
    search_resized_file = None

    if current_size > image_size_limit:
        while not is_in_range(search_size, image_size_limit, error):
            inferior_resized_file = get_scaled_image(pil_image, inferior_scale)
            inferior_size = get_image_size(inferior_resized_file)

            superior_resized_file = get_scaled_image(pil_image, superior_scale)
            superior_size = get_image_size(superior_resized_file)
            
            search_resized_file = get_scaled_image(pil_image, search_scale)
            search_size = get_image_size(search_resized_file)

            if search_size < image_size_limit < superior_size:
                inferior_scale = search_scale
            elif inferior_size < image_size_limit < search_size:
                superior_scale = search_scale

            search_scale = ((superior_scale - inferior_scale) / 2) + inferior_scale

        return search_resized_file

    return pil_image

def is_in_range(value, target, error):
    return (target - error <= value <= target)

def get_scaled_image(pil_image, scale):
    current_size = pil_image.size
    size = (int(current_size[0] * scale), int(current_size[1] * scale))
    resized_image = pil_image.resize(size, Image.ANTIALIAS)
    resized_image.format = pil_image.format
    return resized_image

def get_image_size(pil_image):
    image_bytes = BytesIO()
    pil_image.save(image_bytes, quality=95, format=pil_image.format)
    return image_bytes.getbuffer().nbytes

def main():
    try:
        url_from = sys.argv[1]
        result_filename = sys.argv[2]
        filesize = int(sys.argv[3])
    except IndexError:
        sys.exit("Arguments missing. Use as: python image_file_scaler.py <url_origin> <name_destination> <file_size>")
    
    starting_time = datetime.datetime.now()

    if urlparse(url_from).scheme == "":
        url_from = "http://" + url_from
    if urlparse(url_from).scheme not in ('http', 'https', 'file',):
        url_from = "file:///" + url_from
    
    print(("Process started. Scaling the image until the file size is {0}MB.".format(filesize)))
    result = image_download(url_from, filesize)
    end_time = datetime.datetime.now() - starting_time
    
    result.save(result_filename + r'.' + result.format, quality=95, format=result.format)
    print(("Scaling finished. Time elapsed: {0}.".format(end_time)))

if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
