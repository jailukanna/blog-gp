---
title: PIL example show-me-the-code-github-once-item cn com fangself item item 1 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'show-me-the-code-github-once-item cn com fangself item item 1'


## python show-me-the-code-github-once-item cn com fangself item item 1

Python PIL example: show-me-the-code-github-once-item cn com fangself item item 1

```python
#!/usr/bin/python
#-*-encoding:utf-8-*-

"""
Add a number into your QQ user header Image
"""
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

num = "4"
shuiyin = "draw by http://fangself.com.cn"
img = Image.open("user.png")
width= 150


font_num = ImageFont.truetype("simfang.ttf",50)
font_shui_yin = ImageFont.truetype("simfang.ttf",10)
draw = ImageDraw.Draw(img)
draw.text((120,0),num,font=font_num)
draw.text((0,140),shuiyin,font=font_shui_yin)
img.save("cpy.png","png")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
