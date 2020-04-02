---
title: pil example untitled pattern gen test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'untitled pattern gen test'


## python untitled pattern gen test

Python pil example: untitled pattern gen test

```python
"""
title       : pillow sample code
project     : background pattern generator
filename    : pattern_gen_test.py

description : カラーバーの生成と結合によるパターン画像作成テスト

date        : 2017/07/28
version     : 1.0

reference   : https://librabuch.jp/blog/2013/05/python_pillow_pil/

"""
from PIL import Image

#マージに利用する下地を作成(白塗り) width:1000, height:500
canvas = Image.new('RGB', (1000,500), (255,255,255))

#パターンの各要素を作成
pat_red = Image.new('RGB', (500,500), (255,0,0))
pat_green = Image.new('RGB', (500,500), (0,255,0))

#下地にパターンの要素を貼り付け
canvas.paste(pat_red,(0,0))
canvas.paste(pat_green,(500,0))

#パターン画像生成
canvas.save('pattern_test.jpg', 'JPEG', quality = 100, optimize = True)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
