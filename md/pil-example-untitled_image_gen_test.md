---
title: pil example untitled image gen test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'untitled image gen test'


## python untitled image gen test

Python pil example: untitled image gen test

```python
"""
title       : pillow sample code
project     : background pattern generator
filename    : image_gen_test.py

description : pillowの基本操作と画像ファイル出力テスト

date        : 2017/07/28
version     : 1.0

reference   : https://librabuch.jp/blog/2013/05/python_pillow_pil/

"""
from PIL import Image, ImageDraw, ImageFont

#画像オブジェクト 色形式, 画像サイズ, 背景色を指定
text_canvas = Image.new('RGB', (80,40), (255,255,255))
draw = ImageDraw.Draw(text_canvas)

# フォントの種類とサイズ指定
font = ImageFont.truetype('C:\Windows\Fonts\ipaexg.ttf', 15)

# 指定の座標にテキスト書き込み 書き込み座標, テキスト, フォント, 文字色
# fill には16進のカラーコード
draw.text((10,10), 'hogeほげ', font=font, fill='#000')

#画像として保存 ファイルの名前, 出力形式, 品質(75or100安定?), optimizeはやらなくてもよい?
text_canvas.save('text_img.jpg', 'JPEG', quality = 100, optimize=True)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
