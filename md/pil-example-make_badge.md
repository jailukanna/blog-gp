---
title: pil example make badge (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'make badge'

Functions in program: 
* `def main():`
* `def get_resized_qr(image):`
* `def make_qr_jpg(png_image):`

Modules used in program: 
* `import sys`
* `import os`

## python make badge

Python pil example: make badge

```python
import os
import sys

from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw

def make_qr_jpg(png_image):
    bg = Image.new("RGB", png_image.size, (0, 0, 0))
    bg.paste(png_image, png_image)
    bg.save('qr.jpg')
    return

def get_resized_qr(image):
    size = 400, 400
    return image.resize(size, Image.ANTIALIAS)

def main():
    try:
        directory_name = sys.argv[1]
    except:
        print("Please specify the directory name")

    destination_dir = 'badges_with_names'
    os.mkdir(destination_dir)

    W, H = (1200, 1800)

    files = os.listdir(directory_name)
    for a_file in files:
        name = a_file.split('_')[0].title()
        ticket_no = a_file.split('_')[1][:-4]
        badge = Image.open('badge.jpg')
        qr_png = Image.open(directory_name+"/"+a_file)

        make_qr_jpg(qr_png)

        qr_jpg = Image.open('qr.jpg')
        qr_jpg = get_resized_qr(qr_jpg)
        badge.paste(qr_jpg, (400, 400), 0)

        draw = ImageDraw.Draw(badge)
        font_name = ImageFont.truetype('/usr/share/fonts/truetype/freefont/FreeSerif.ttf', 75)
        font_ticket = ImageFont.truetype('/usr/share/fonts/truetype/freefont/FreeSerif.ttf', 45)

        # Write the name
        w, h = draw.textsize(name, font_name)
        draw.text(((W - w) / 2, 800), name, (0, 0, 0), font=font_name)

        # Write the ticket Number
        w, h = draw.textsize(ticket_no, font_ticket)
        draw.text(((W - w) / 2, 900), ticket_no, (0, 0, 0), font=font_ticket)

        destination_file = destination_dir + "/" + a_file[:-4] + ".jpg"
        badge.save(destination_file)


if __name__=='__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
