---
title: pil example imagedit (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'imagedit'


## python imagedit

Python pil example: imagedit

```python
>>> from PIL import Image
>>> image = Image.open("~/Pictures/artwork/imagine-copy.jpg")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/local/lib/python2.7/dist-packages/PIL/Image.py", line 2258, in open
    fp = builtins.open(filename, "rb")
IOError: [Errno 2] No such file or directory: '~/Pictures/artwork/imagine-copy.jpg'
>>> image = Image.open("/home/ray/Pictures/artwork/imagine-copy.jpg")
>>> image
<PIL.JpegImagePlugin.JpegImageFile image mode=RGB size=1300x1300 at 0x7F5FF5A42690>
>>> image.format
'JPEG'
>>> image.mode
'RGB'
>>> image.size
(1300, 1300)
>>> image.show()
>>> image.save("/home/ray/Pictures/artwork/imagine-copy.png")
>>> cropped = image.crop((1, 1, 1299, 1299))
>>> cropped.save("/home/ray/Pictures/artwork/imagine-copy-cropped.png")
>>> cropped.resize((640, 640), resample=PIL.Image.ANTIALIAS)
<PIL.Image.Image image mode=RGB size=640x640 at 0x7F5FF5A42E50>
>>> resized = cropped.resize((640, 640), resample=PIL.Image.ANTIALIAS)
>>> resized.save("/home/ray/Pictures/artwork/imagine-copy-cropped-resized.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
