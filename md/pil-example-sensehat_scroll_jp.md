---
title: pil example sensehat scroll jp (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'sensehat scroll jp'

Functions in program: 
* `def show_scroll_string(disp_str, fore_color = "#ffffff", back_color = "#000000", interval = 0.1):`
* `def count_byte(s):`

Modules used in program: 
* `import sys`
* `import time`
* `import unicodedata`

## python sensehat scroll jp

Python pil example: sensehat scroll jp

```python
# -*- encoding:utf8 -*-
import unicodedata
import time
import sys
from PIL import Image, ImageDraw, ImageFont
from sense_hat import SenseHat

# 文字列の総バイト数を取得
def count_byte(s):
    n = 0
    for c in s:
        wide_chars = u"WFA"
        eaw = unicodedata.east_asian_width(c)
        if wide_chars.find(eaw) > -1:
            n += 2
        else:
            n += 1
    return n

# SenseHATにスクロール表示
def show_scroll_string(disp_str, fore_color = "#ffffff", back_color = "#000000", interval = 0.1):
    '''
        disp_str   = 表示させる文字列
        fore_color = 文字色（省略可）
        back_color = 背景色（省略可）
        interval   = スクロール表示間隔（省略可）
    '''
    matrix_size = 8
    img_width = (count_byte(disp_str) * 4) + (matrix_size * 2)

    sense = SenseHat()
    font = ImageFont.truetype("/misaki_gothic.ttf", # フォントのパスを指定
                              8, encoding="unic")
    img_moto = Image.new("RGB", (img_width, matrix_size), back_color)
    draw = ImageDraw.Draw(img_moto)
    draw.text((matrix_size, 1), disp_str, font = font, fill = fore_color)
    img_moto.save("./img.png", "PNG")

    for num in range(0, img_width - matrix_size):
        img = img_moto.crop((num, 0, matrix_size + num, matrix_size))
        img.save("./img_.png", "PNG")
        sense.load_image("./img_.png")
        time.sleep(interval)
    sense.clear()

# main
show_scroll_string(u"文字列もじれつMojiretsu。", interval = 0.07)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
