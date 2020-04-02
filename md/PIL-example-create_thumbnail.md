---
title: PIL example create thumbnail (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'create thumbnail'

Functions in program: 
* `def create_thumbnail(sender, **kwargs):`

## python create thumbnail

Python PIL example: create thumbnail

```python
@receiver(pre_save, sender=Photo)
def create_thumbnail(sender, **kwargs):
    instance = kwargs.pop('instance')

    if not instance.image:
        return

    from PIL import Image
    from io import BytesIO
    from django.core.files.uploadedfile import SimpleUploadedFile

    thumb_size = (200,200)

    if instance.image.name.endswith(".jpg"):
        PIL_TYPE = 'jpeg'
        DJANGO_TYPE = 'image/jpeg'
        FILE_EXT = 'jpg'
    elif instance.image.name.endswith(".png"):
        PIL_TYPE = 'png'
        DJANGO_TYPE = 'image/png'
        FILE_EXT = 'png'

    image = Image.open(BytesIO(instance.image.read()))
    image.thumbnail(thumb_size, Image.ANTIALIAS)

    fp = BytesIO()
    image.save(fp, PIL_TYPE)
    fp.seek(0)

    suf = SimpleUploadedFile(os.path.split(instance.image.name)[-1], fp.read(), content_type=DJANGO_TYPE)
    instance.thumb.save('{}_thumb.{}'.format(os.path.splitext(suf.name)[0], FILE_EXT), suf, save=False)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
