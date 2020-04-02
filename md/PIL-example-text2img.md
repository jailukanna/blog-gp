---
title: PIL example text2img (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'text2img'

Functions in program: 
* `def list2img(list_items, file_name=None):`
* `def text2img(text, file_name="text_to_png.png", default_width=300,`

Modules used in program: 
* `import PIL`

## python text2img

Python PIL example: text2img

```python
# -*- coding: utf-8 -*-s
import PIL
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
NEW_LINE_MARK = "\n"


def text2img(text, file_name="text_to_png.png", default_width=300,
             bgcolor="#FFF", color="#000", padding=10):
    "Convert um texto para uma imagem PB"

    font = ImageFont.load_default().font
    lines = text.split(NEW_LINE_MARK)

    total_lines = len(lines)
    first_line = lines[0] if lines else default_width
    width, height = font.getsize(first_line)

    padding_board = (padding * 2)
    img_height = (height * (total_lines - 1)) + padding_board
    img_width = width + padding_board

    print("Image Size: %s, %s" % (img_width, img_height))

    img = Image.new("L", (img_width, img_height), bgcolor)
    draw = ImageDraw.Draw(img)

    y = padding
    for line in lines:
        if line:
            draw.text((padding, y), line, color, font=font)
            y += height

    img.save(file_name)


def list2img(list_items, file_name=None):
    "Converte uma lista para um imagem PB"

    text = ""
    for i in list_items:
        text += ", ".join(i)
        text += NEW_LINE_MARK

    print(text)

    text2img(text, file_name)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
