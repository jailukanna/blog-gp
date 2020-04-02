---
title: PIL example screen dialog (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'screen dialog'

Functions in program: 
* `def test():`
* `def calculate_size(text):`

Modules used in program: 
* `import datetime, io`

## python screen dialog

Python PIL example: screen dialog

```python
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont
from PIL import ImageOps

import datetime, io

f_s = 13

font1 = ImageFont.truetype("roboto.ttf", f_s)
font2 = ImageFont.truetype("roboto_t.ttf", f_s)

def calculate_size(text):
    textsss = text.split("\n")
    minw = 0
    maxh = 0 
    for x in textsss:
        if x == "":
            x = "1" 
        fsd = font1.getsize(x)
        if fsd[0] > minw:
            minw = fsd[0]
        maxh+=fsd[1]
    
    return (minw, maxh)


class Message:

    def __init__(self, avatar, time, text, name):
        self.name = name
        self.avatar = avatar
        self.time = time
        self.text = text
        self.img = None

    def calc_image(self):
        font_size1 = font1.getsize(self.name)
        font_size15 = font1.getsize("00:00  ")
        font_size2 = font1.getsize(self.text) if not self.text.find("\n") != -1 else calculate_size(self.text)

        img = Image.new('RGB', (200+32+font_size2[0]+font_size15[0], 20+18+font_size1[1]+font_size2[1]), (255, 255, 255))

        avatar = Image.open(self.avatar).resize((42,42)) if type(self.avatar) == str else self.avatar.resize((42, 42))
        ll_size = (1000, 1000)
        mask = Image.new('L', ll_size, 0)
        draw = ImageDraw.Draw(mask)
        draw.ellipse((0, 0) + ll_size, fill=255)

        mask = ImageOps.fit(mask, avatar.size, method=Image.BICUBIC, centering=(0.5, 0.5))
        avatar.putalpha(mask)

        img.paste(avatar, (f_s, f_s), avatar)
        draw = ImageDraw.Draw(img)

        draw.text((f_s+42+10,f_s), self.name, font=font1, fill=(66, 100, 139))
        draw.text((f_s+42+15+font_size1[0],f_s), self.time, font=font2, fill=(120,127,140))
        draw.text((f_s+42+10,f_s+13+6), self.text, font=font2, fill=(0,0,0))

        self.img = img

messages = [
    Message("ava.jpg", "20:38", "пхп говно", "Tanya Kruit"),
    Message("ava2.jpg", "20:38", "пхп это я", "Глеб Фараонов")
]

width = 0
height = 0

for msg in messages:
    msg.calc_image()
    
    if width < msg.img.size[0]:
        width = msg.img.size[0]
    
    height+=msg.img.size[1]

img = Image.new('RGB', (width, height), (255,255,255))
last_pos_y = 0 
for msg in messages:
    img.paste(msg.img, (0, last_pos_y))
    last_pos_y+=msg.img.size[1]-f_s+4


def test():
    img.save("test.jpg", format="JPEG")

test()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
