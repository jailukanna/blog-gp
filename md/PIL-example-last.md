---
title: PIL example last (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'last'

Functions in program: 
* `def scanstars(stars,img,scale):`

Modules used in program: 
* `import numpy as np`
* `import random`
* `import cv2`

## python last

Python PIL example: last

```python
# -*- coding: utf-8 -*-

import cv2
import random
import numpy as np
from make_star_image import make_star_image

from PIL import Image


#入力ベースマップ
#作りたい元の画像
# 出力星の数
def scanstars(stars,img,scale):

    width, height = img.shape[:2]
    ratio = 10.0 * scale/width

    x = random.random() * 360
    y = random.random() * 180

    out_img = make_star_image(stars)
    H,W,C = out_img.shape

    dst = np.zeros(( H, W, 3), np.uint8)
    dstMask = np.zeros(( H, W, 1), np.uint8)
    akari = img
    h, w, c = akari.shape
    akari = cv2.resize( akari,(int(h/3),int(w/3)))

    print(akari.shape)

    akarin = akari[:,:,:3]

    cv2.imwrite("aka.png", akarin)
    #np.copyto(dst[100:100+int(w/3),100:100+int(h/3),0:3], akarin);
    dst    [100:100+int(w/3), 100:100+int(h/3), 0:3] = akarin
    dstMask[100: 100+int(w/3), 100:100+int(h/3), 0] = akari[:,:,3]

    dstMask = cv2.cvtColor(dstMask, cv2.COLOR_GRAY2RGB)

    cv2.imwrite("hoge.png", dst)
    out_img2 = out_img[:,:,:3]

    dst *= 254 - dstMask
    #dst += 1

    out_img2[0:H, 0:W] += dst

    #out_img[0:height:, 0:width] += out_img *
    cv2.imwrite('star_with_akari.png', out_img2)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
