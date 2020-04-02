---
title: pil example image exif (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'image exif'


Modules used in program: 
* `import PIL.ExifTags`
* `import PIL.Image as img`

## python image exif

Python pil example: image exif

```python
# Extract EXIF data from image files using pillow 

import PIL.Image as img
import PIL.ExifTags

file_path = 'sample.jpg'
file = img.open(file_path)
data = file._getexif()

exif = {
    PIL.ExifTags.TAGS[k]: v
    for k, v in data.items()
    if k in PIL.ExifTags.TAGS
}

print(exif)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
