---
title: PIL example utils (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'utils'

Functions in program: 
* `def download_link_picture(img_url: str):`

Modules used in program: 
* `import webp`
* `import user_agents`
* `import requests`
* `import arrow`
* `import secrets`
* `import re`
* `import os`

## python utils

Python PIL example: utils

```python
import os
import re
import secrets
from io import BytesIO
from pprint(import pprint)
from urllib.parse import urlsplit

import arrow
import requests
import user_agents
from PIL import Image, ImageOps
import webp

def download_link_picture(img_url: str):
    """
    Download image from url (support WebP)
    """
    import cairosvg
    now = arrow.now().to(settings.TIME_ZONE)
    path_ = os.path.join(settings.MEDIA_ROOT, 'covers/{year}-{month}/'.format(year=now.format('YYYY'), month=now.format('MM')))
    if not os.path.exists(path_):
        os.makedirs(path_)
    fn = os.path.join(path_, '%s' % now.timestamp)
    try:
        r = requests.head(img_url)
    except (requests.exceptions.InvalidURL,
            requests.exceptions.MissingSchema,
            requests.exceptions.InvalidSchema,
            requests.exceptions.ConnectionError):
        return
    if r.status_code == 200:
        r = requests.get(img_url)
        ct = r.headers['Content-Type'].split('/')[-1]
        ct = 'JPEG' if ct.lower() == 'jpg' else ct
        if r.ok:
            if '<svg' in r.text:
                fn = fn + '.png'
                cairosvg.svg2png(bytestring=r.content, output_width=512, write_to=fn)
                return fn
            elif ct == 'webp':
                o = Image.open(BytesIO(r.content))
                pic = webp.WebPPicture.from_pil(o)
                config = webp.WebPConfig.new(preset=webp.WebPPreset.PHOTO, quality=90)
                buf = pic.encode(config).buffer()
                webp.save_image(buf, fn + '.webp')
            else:
                o = Image.open(BytesIO(r.content))
                if o.size[0] < 200:
                    o = ImageOps.fit(o, (200, 300), Image.ANTIALIAS, 0, (0.5, 0.5))
                if o.format == 'GIF':
                    # ảnh GIF convert quá PNG, lấy frame đầu tiên
                    from PIL import ImageSequence
                    frames = [frame.copy() for frame in ImageSequence.Iterator(o)]
                    # write GIF animation
                    fn = fn + '.png'
                    o.save(fn, 'png', append_images=frames, quality=90, **o.info)
                    return fn
                elif o.format == 'BMP':
                    # ảnh Bitmap convert qua JPEG
                    fn = fn + '.jpg'
                    o = o.convert('RGB')
                    o.save(fn, 'jpeg', quality=90, **o.info)
                    return fn
                else:
                    if o.mode == 'P':
                        o = o.convert('RGB')
                    elif o.mode in ('RGBA', 'LA'):
                        background = Image.new(o.mode[:-1], o.size, '#ffffff')
                        background.paste(o, o.split()[-1])
                        o = background
                    png_info = o.info
                    ext = 'jpg' if ct.lower() == 'jpeg' else ct.lower()
                    fn = fn + '.' + ext
                    o.save(fn, ct, quality=90, **png_info)
                    return fn
    return


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
