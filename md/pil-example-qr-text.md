---
title: pil example qr-text (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'qr-text'

Functions in program: 
* `def main():`
* `def concat_vertical(image1, image2):`

Modules used in program: 
* `import qrcode`
* `import sys`

## python qr-text

Python pil example: qr-text

```python
import sys
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw
import qrcode

def concat_vertical(image1, image2):
    w1, h1 = image1.size
    w2, h2 = image2.size
    w = max(w1, w2)
    h = h1 + h2
    ret = Image.new('RGB', (w, h))
    ret.paste(image1, (0, 0))
    ret.paste(image2, (0, h1))
    return ret

def main():
    text = "www.google.com"
    qr_i = qrcode.make(text)

    text_i = Image.new('RGB', qr_i.size)
    draw = ImageDraw.Draw(text_i)
    font = ImageFont.truetype("Arial.ttf", 16)
    draw.text((0, 0), text, (255, 255, 255), font=font)

    final = concat_vertical(qr_i, text_i)
    final.save('test.jpg')

if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
