---
title: pil example pil images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'pil images'


## python pil images

Python pil example: pil images

```python
from weasyprint(import images as weasyprint_images)

# Only monkey-patch if GDK failed to import
if not hasattr(weasyprint_images, 'PixbufLoader'):
    from io import BytesIO
    try:
        from PIL import Image
    except ImportError:
        # Yeah, PIL packaging.
        import Image

    def pil_loader(file_obj, string):
        if string is None:
            # PIL wants to seek, but file_obj could be eg. a socket.
            string = file_obj.read()
        file_obj = BytesIO(string)
        png_file = BytesIO()
        Image.open(file_obj).save(png_file, 'PNG')
        png_file.seek(0)
        return weasyprint_images.cairo_png_loader(png_file, None)

    weasyprint_images.gdkpixbuf_loader = pil_loader

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
