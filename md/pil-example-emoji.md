---
title: pil example emoji (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'emoji'

Functions in program: 
* `def main(img, text=()):`
* `def generate(img, text):`

Modules used in program: 
* `import textwrap`
* `import uuid`
* `import sys`

## python emoji

Python pil example: emoji

```python
import sys
import uuid
import textwrap

from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw


def generate(img, text):
    # open image
    i = Image.open(img)

    # calc target size
    base_width = 160
    percent = base_width / float(i.size[0])
    target_height = int((float(i.size[1]) * float(percent)))
    i = i.resize((base_width, target_height), Image.ANTIALIAS)
    draw = ImageDraw.Draw(i)

    # paragraph
    para = textwrap.wrap(text, width=8)

    # font
    font = ImageFont.truetype("PingFang.ttc", 16)

    # draw text
    vertical_center_offset = 10
    current_height = target_height / 2.0 + vertical_center_offset
    line_padding = 2
    for line in para:
        w, h = draw.textsize(line, font=font)
        draw.text(((base_width - w) / 2.0, current_height), line, (255, 0, 0), font=font)
        current_height += h + line_padding

    i.save(str(uuid.uuid4()) + '.png')


def main(img, text=()):
    for t in text:
        generate(img, t.decode('utf-8'))


if __name__ == '__main__':
    main(sys.argv[1], tuple(sys.argv[2:]))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
