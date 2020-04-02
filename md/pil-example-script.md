---
title: pil example script (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'script'

Functions in program: 
* `def add_watermark(image_file, logo_file, opacity=.8):`

Modules used in program: 
* `import StringIO`
* `import PIL as pil`
* `import json`

## python script

Python pil example: script

```python
# -*- coding: utf-8 -*-
import json
from datetime import datetime
from itertools import izip
from decimal import Decimal
import PIL as pil
from PIL import ImageEnhance
import StringIO
from django.contrib.auth import update_session_auth_hash

from django.shortcuts import render, redirect
from django.core.urlresolvers import reverse
from django.db.models import (
    Prefetch, Count, Case, When, IntegerField, Sum, DecimalField, Q)
from django.db.models.functions import Coalesce, Concat
from django.http import Http404, JsonResponse, HttpResponse
from django.conf import settings
from django.core.files.base import ContentFile


def add_watermark(image_file, logo_file, opacity=.8):
    img = pil.Image.open(image_file).convert('RGB')

    logo = pil.Image.open(logo_file)

    # TODO: add aspect ratio as an argument
    base_width = img.size[0] / float(2) * 2
    base_height = img.size[1] / float(3) * 2.5

    base_width = min(base_width, base_height)
    wpercent = base_width / float(logo.size[0])
    hsize = int((float(logo.size[1]) * float(wpercent)))
    logo = logo.resize((int(base_width), hsize), pil.Image.ANTIALIAS)

    # position the watermark
    offset_x = abs(img.size[0] - logo.size[0]) / 2
    offset_y = abs(img.size[1] - logo.size[1]) / 2

    watermark = pil.Image.new('RGBA', img.size, (255, 255, 255, 1))
    watermark.paste(logo, (offset_x, offset_y))

    alpha = watermark.split()[3]
    alpha = pil.ImageEnhance.Brightness(alpha).enhance(opacity)

    watermark.putalpha(alpha)
    pil.Image.composite(
            watermark, img, watermark
    ).save(".".join(image_file.path.split(".")[:-1]) + ".jpg", 'JPEG')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
