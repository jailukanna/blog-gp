---
title: PIL example pdf2bwPngs (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pdf2bwPngs'


Modules used in program: 
* `import PIL.ImageOps    `
* `import pdf2image`
* `import glob, os`

## python pdf2bwPngs

Python PIL example: pdf2bwPngs

```python
import glob, os
import pdf2image
from PIL import Image, ImageEnhance 
import PIL.ImageOps    

file = input("File:")
dpi = int(input("DPI:"))
i = 0;
dir = "d_"+file
if not os.path.exists(dir):
    os.makedirs(dir)
maxPages = 100
print("Extracting PNGs")
for _page in range(1,maxPages,2) : 
    pages = pdf2image.convert_from_path(file, dpi=dpi, first_page=_page, last_page = min(_page+2-1,maxPages))
    for page in pages:
        i+=1
        page.save(os.path.join(dir, str(i)+".png"), 'PNG')

if not os.path.exists(dir):
    os.makedirs(dir)
print("Increasing contrast and converting")
for file in os.listdir(dir):
    if file.endswith(".png"):
        img = Image.open(os.path.join(dir, file)).convert("RGB", colors=16)
        enhancer = ImageEnhance.Contrast(img)
        img = enhancer.enhance(10)
        enhancer = ImageEnhance.Brightness(img)
        img = enhancer.enhance(1.2)
        img = img.convert("L")
        
        dark_count = 0;
        light_count = 0;
        for pixel in img.getdata():
            if(pixel < 128):
                dark_count += 1
            else:
                light_count += 1
        if(dark_count > light_count):
            img = PIL.ImageOps.invert(img)
            print("Inverted: "+file);
        img.save(os.path.join(dir, file),'png')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
