---
title: PIL example test plugins (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'test plugins'

Functions in program: 
* `def coverart_download_processor(coverartimage):`
* `def coverart_tag_processor(coverartimage):`
* `def coverart_file_processor(coverartimage):`

## python test plugins

Python PIL example: test plugins

```python
# -*- coding: utf-8 -*-

PLUGIN_NAME = u"Cover Art Processors"
PLUGIN_AUTHOR = u"Sambhav Kothari"
PLUGIN_DESCRIPTION = ""
PLUGIN_VERSION = "0.1"
PLUGIN_API_VERSIONS = ["0.15"]
from picard.coverart import (
    register_coverart_downloaded_action,
    register_coverart_file_save_action,
    register_coverart_tag_embed_action)
from picard import log


def coverart_file_processor(coverartimage):
    try:
        from PIL import Image, ImageOps
        import io
    except ImportError:
        log.debug("Pillow not found. Please make sure Pillow(Python) is installed to allow\nresizing and cropping coverart images.")
    else:
        resize_values = (200, 200)
        if coverartimage.width > resize_values[0] or coverartimage.height > resize_values[1]:
            im = Image.open(io.BytesIO(coverartimage.data))
            im = ImageOps.fit(im, resize_values)
            image_type = coverartimage.mimetype.split("/")[1]
            store_buffer = io.BytesIO()
            im.save(store_buffer, image_type)
            coverartimage.set_data(store_buffer.getvalue())
        log.debug("Resized images.")


def coverart_tag_processor(coverartimage):
    try:
        from PIL import Image, ImageOps
        import io
    except ImportError:
        log.debug("Pillow not found. Please make sure Pillow(Python) is installed to allow\nresizing and cropping coverart images.")
    else:
        resize_values = (200, 200)
        if coverartimage.width > resize_values[0] or coverartimage.height > resize_values[1]:
            im = Image.open(io.BytesIO(coverartimage.data))
            im = ImageOps.fit(im, resize_values)
            image_type = coverartimage.mimetype.split("/")[1]
            store_buffer = io.BytesIO()
            im.save(store_buffer, image_type)
            coverartimage.set_data(store_buffer.getvalue())
        log.debug("Resized images.")


def coverart_download_processor(coverartimage):
    try:
        from PIL import Image, ImageOps
        import io
    except ImportError:
        log.debug("Pillow not found. Please make sure Pillow(Python) is installed to allow\nresizing and cropping coverart images.")
    else:
        resize_values = (200, 200)
        if coverartimage.width > resize_values[0] or coverartimage.height > resize_values[1]:
            im = Image.open(io.BytesIO(coverartimage.data))
            im = ImageOps.fit(im, resize_values)
            image_type = coverartimage.mimetype.split("/")[1]
            store_buffer = io.BytesIO()
            im.save(store_buffer, image_type)
            coverartimage.set_data(store_buffer.getvalue())
        log.debug("Resized images.")


register_coverart_file_save_action(coverart_file_processor)
register_coverart_tag_embed_action(coverart_tag_processor)
register_coverart_downloaded_action(coverart_download_processor)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
