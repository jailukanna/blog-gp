---
title: pil example font test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'font test'

Functions in program: 
* `def inputJP(name, img, text, x, y, size, color, output):`

Modules used in program: 
* `import cv2`
* `import numpy as np`

## python font test

Python pil example: font test

```python
from PIL import ImageFont, ImageDraw, Image
import numpy as np
import cv2

# 設定パラメータ
WINDOW_NAME = "test"
WINDOW_WIDTH = 1024
WINDOW_HEIGHT = 768

# ベース画像の準備
base = np.zeros((WINDOW_HEIGHT, WINDOW_WIDTH, 3), dtype=np.uint8)
base.fill(55)

# フォントパスの設定
fontpath = 'font/APJapanesefont.ttf'

def inputJP(name, img, text, x, y, size, color, output):
	font = ImageFont.truetype(fontpath, size)
	img_pil = Image.fromarray(img)
	draw = ImageDraw.Draw(img_pil)

	draw.text((x, y), text, font=font, fill=color)
	output_img = np.array(img_pil)

	if output == 1:
		cv2.imshow(name, output_img)
		cv2.waitKey(1)

	return output_img
	

if __name__ == '__main__':
	# ウィンドウの準備
	cv2.namedWindow(WINDOW_NAME)
	cv2.moveWindow(WINDOW_NAME, 0, 0)	

	cv2.imshow(WINDOW_NAME, base)
	temp = inputJP(WINDOW_NAME, base, '艦隊これくしょん\nhoge', 200, 200, 100, '#fff', 0)
	temp = inputJP(WINDOW_NAME, temp, 'Fleet Collection', 220, 300, 100, '#fff', 0)
	temp = inputJP(WINDOW_NAME, temp, '艦これアーケード', 220, 400, 100, '#fff', 0)
	temp = inputJP(WINDOW_NAME, temp, '1234567890', 280, 500, 100, '#fff', 1)

	cv2.waitKey(0)

	# 終了処理
	cv2.destroyAllWindows()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
