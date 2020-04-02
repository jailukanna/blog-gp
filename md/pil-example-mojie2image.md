---
title: pil example mojie2image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'mojie2image'


Modules used in program: 
* `import PIL.ImageFont`
* `import PIL.ImageDraw`
* `import PIL.Image`
* `import numpy as np`

## python mojie2image

Python pil example: mojie2image

```python
#-*- coding: utf-8 -*-
import numpy as np

import PIL.Image
import PIL.ImageDraw
import PIL.ImageFont


class TextDraw:

    def __init__(self):
        FONT_PATH = "/Library/Fonts/Microsoft/MS Gothic.ttf"
        self.FONT_SIZE = 20
        self.MARGIN = 20
        self.font = PIL.ImageFont.truetype(FONT_PATH, self.FONT_SIZE)
        self.TEXT_COLOR = 'white'
        self.BG_COLOR = (0, 40, 40)  # 黒板色

    def compute_image_size(self, text):
        line_num = len(text.splitlines())
        sizes = np.zeros((line_num, 2), dtype=np.int)
        for (i, line) in enumerate(text.splitlines()):
            sizes[i] = self.font.getsize(line)
        width = np.max(sizes) + self.MARGIN
        height = (self.FONT_SIZE * line_num) + (self.MARGIN * 2)
        size = (width, height)
        return size

    def draw(self, text, img):
        draw = PIL.ImageDraw.Draw(img)
        draw.font = self.font
        for (i, line) in enumerate(text.splitlines()):
            x = self.MARGIN / 2
            y = (i * self.FONT_SIZE) + self.MARGIN
            pos = (x, y)
            draw.text(pos, line, self.TEXT_COLOR)

    def run(self, text, filename=None):
        img_size = self.compute_image_size(text)
        img = PIL.Image.new("RGBA", img_size, self.BG_COLOR)
        self.draw(text, img)
        img.show()
        if filename:
            img.save(filename)

if __name__ == '__main__':
    text = u"""
たぬぽこ(´ー｀)ノ
　　　　 (ヽ　)"""
    drawer = TextDraw()
    drawer.run(text)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
