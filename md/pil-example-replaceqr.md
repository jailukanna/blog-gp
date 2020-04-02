---
title: pil example replaceqr (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'replaceqr'


Modules used in program: 
* `import qrcode`
* `import zbar`

## python replaceqr

Python pil example: replaceqr

```python
#!/usr/bin/python
from sys import argv
import zbar
import qrcode
from PIL import Image

if len(argv) < 2: exit(1)
newUrl = argv[2]
scanner = zbar.ImageScanner()

scanner.parse_config('enable')

pil = Image.open(argv[1]).convert('L')
width, height = pil.size
raw = pil.tobytes()

image = zbar.Image(width, height, 'Y800', raw)

scanner.scan(image)
originCopy = pil.copy();
# for every symbol in image caculate width and height ,generate the qrcode and resize it , paste the generated qrcode to origin location
for symbol in image:
    qrWidth = bottomRightCorners[0] - topLeftCorners[0]
    qr = qrcode.QRCode(
        border = 0
    )
    qr.add_data(newUrl)
    qr.make(fit=True)
    image = qr.make_image();
    resizedImage = image.resize((qrWidth, qrHeight))

    originCopy.paste(resizedImage, topLeftCorners);

originCopy.save("new.png");
del(image)
#usage python replaceqr.py http://kymtwyf.github.io

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
