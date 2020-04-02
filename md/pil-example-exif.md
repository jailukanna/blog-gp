---
title: pil example exif (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'exif'


Modules used in program: 
* `import PIL.ExifTags`
* `import PIL.Image`

## python exif

Python pil example: exif

```python
import PIL.Image
img = PIL.Image.open('img.jpg')
exif_data = img._getexif()


import PIL.ExifTags
exif = {
    PIL.ExifTags.TAGS[k]: v
    for k, v in img._getexif().items()
    if k in PIL.ExifTags.TAGS
}

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
