---
title: pil example pil image tip (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'pil image tip'


Modules used in program: 
* `import numpy as np`

## python pil image tip

Python pil example: pil image tip

```python
#!/usr/bin/python
#-*- coding: utf-8 -*-

from PIL import Image
import numpy as np

#　read Image
im = Image.open('../inputs/001.png')
im.show()

# gammas_output
im = np.array(Image.open('../inputs/001.png'), 'f')  # floatで読み込み
im_1_22 = 255.0 * (im/255.0)**(1/2.2)
im_22 = 255.0 * (im/255.0)**2.2
im_gamma = np.concatenate((im_1_22, im, im_22), axis=1)
pil_img = Image.fromarray(np.uint8(im_gamma))  # floatをuintに変換
pil_img.save('../outputs/lena_gammas.jpg')

# get rgb
im = np.array(Image.open('../inputs/001.png'))
# 次元数、サイズ
print(im.ndim, im.shape)
# => 3 (512, 512, 3)
# 指定した座標の色（原点は左上）
print(im[216, 216])
# => [181  66  73]
# Redの最小値
print(im[:, :, 0].min())
# => 50
im_R = im.copy()
im_R[:, :, (1, 2)] = 0
im_G = im.copy()
im_G[:, :, (0, 2)] = 0
im_B = im.copy()
im_B[:, :, (0, 1)] = 0
# 横に並べて結合（どれでもよい）
im_RGB = np.concatenate((im_R, im_G, im_B), axis=1)
# im_RGB = np.hstack((im_R, im_G, im_B))
# im_RGB = np.c_['1', im_R, im_G, im_B]
pil_img = Image.fromarray(im_RGB)
pil_img.save('../outputs/lena_RGBs.jpg')

# to csv
# テキストデータ読み込み
data = np.loadtxt("../inputs/front_table.csv", delimiter=",")
# 行列の転置
t_data = data.T
# 一行にまとめる
result_list = []
for line in t_data:
	for element in line:
		result_list.extend([element])
#文字列から作成する場合
data_result = np.array(result_list, dtype=np.uint8)
data_result = data_result.reshape(960,1080)
image = Image.fromarray(data_result)
image.save('front_table_after.png')

# import csv
f = open('../inputs/data.csv', 'r')
dataReader = csv.reader(f)
for row in dataReader:
	print(row)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
