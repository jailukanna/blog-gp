---
title: PIL example kao (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'kao'

Functions in program: 
* `def get_images_and_labels(path):`

Modules used in program: 
* `import glob`
* `import sys`
* `import os`
* `import numpy as np`
* `import cv2, os`

## python kao

Python PIL example: kao

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

import cv2, os
import numpy as np
from PIL import Image
import os
import sys
import glob

# 元画像
train_path = sys.argv[1]
output_path = 'face/'
os.makedirs(output_path, exist_ok=True)

# アニメ顔特徴分類器
cascadePath = "lbpcascade_animeface.xml"
faceCascade = cv2.CascadeClassifier(cascadePath)

# 指定されたpath内の画像を取得
def get_images_and_labels(path):
    # 画像を格納する配列
    images = []
    # ラベルを格納する配列
    labels = []
    # ファイル名を格納する配列
    files = []
    i = 0

    lists = glob.glob(path + '/**/*')
    #print(lists)

    #for f in os.listdir(path):
    for f in lists:
        # 画像のパス
        #image_path = os.path.join(path, f)
        image_path = f
        # 画像を読み込む
        image_pil = Image.open(image_path)
        if image_pil is None:
            continue
        # NumPyの配列に格納
        image = np.array(image_pil, 'uint8')
        if len(image.shape) != 3:
            continue
        # アニメ顔特徴分類器で顔を検知
        faces = faceCascade.detectMultiScale(image)
        # 検出した顔画像の処理
        for (x, y, w, h) in faces:
            # 顔をリサイズ
            roi = cv2.resize(image[y: y + h, x: x + w], (64, 64), interpolation=cv2.INTER_LINEAR)
            # 画像を配列に格納
            images.append(roi)
            # ファイル名を配列に格納
            subdir = os.path.basename(os.path.dirname(f))
            files.append(f)
            save_path = output_path + subdir + str(i) + '.jpg'
            print(save_path + ' from ' + f)
            # そのまま保存すると青みがかる（RGBになっていない）
            cv2.imwrite(save_path, roi[:, :, ::-1].copy())
            #print(i)

            i+=1

    return images

if __name__ == '__main__':
    if len(sys.argv) == 2:
        images = get_images_and_labels(train_path)
    else:
        print('arguments error')

# 終了処理
cv2.destroyAllWindows()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
