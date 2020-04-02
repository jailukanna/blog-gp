---
title: PIL example generate thumbnails (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'generate thumbnails'

Functions in program: 
* `def generate_thumbnails(fn):`

Modules used in program: 
* `import PIL`
* `import re`

## python generate thumbnails

Python PIL example: generate thumbnails

```python
import re
import PIL
from PIL import Image
def generate_thumbnails(fn):
    match = re.match(r"^(?P<path>[^/]+)/receipt_(?P<dimensions>\d{4}x\d{4})_(?P<mp>.{3,5})\.jpg$", fn)
    if not match:
        return 
    
    print("Generating for " + fn)
    match = match.groupdict()
    im = Image.open(fn)
    ds = [0.2, 0.3, 0.4, 0.5, 0.6, 0.7]
    qs = [20, 30, 40, 50]
    cs = [{"d": d, "q": q} for d in ds for q in qs]
    
    for c in cs:
        c.update({"path": match['path'], "mp": match['mp']})
        im.resize(map(lambda x: int(x*c['d']), im.size), PIL.Image.ANTIALIAS).save("%(path)s/receipt_%(mp)s_%(d)sD_%(q)sQ.jpg" % c, "JPEG", quality=c['q'])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
