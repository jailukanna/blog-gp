---
title: PIL example addImageSizeToIMGTag (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'addImageSizeToIMGTag'


Modules used in program: 
* `import io`
* `import glob`

## python addImageSizeToIMGTag

Python PIL example: addImageSizeToIMGTag

```python
#!/bin/python

from BeautifulSoup import BeautifulSoup
from os.path import basename, splitext
from PIL import Image
import glob
import io

path = "/ruta/ficheros/*.markdown"

for fname in glob.glob(path):
    with io.open(fname,'r',encoding='utf8') as f:
        post = f.read()

    soup = BeautifulSoup(post)

    for img in soup.findAll('img'):
        if img != None:
            try:
                if img['src'].startswith("/assets") == True:
                    pil = Image.open("/ruta/imagenes" + img['src'])
                    width, height = pil.size
                    img['width'] = str(width) + "px"
                    img['height'] = str(height) + "px"
            except KeyError:
                pass

    with open(fname, "wb") as file:
        file.write(soup.prettify())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
