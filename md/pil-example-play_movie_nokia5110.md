---
title: pil example play movie nokia5110 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'play movie nokia5110'

Functions in program: 
* `def readVideo(filename):`

Modules used in program: 
* `import time`
* `import threading`
* `import pcd8544.lcd as lcd`
* `import sys`
* `import cv2`
* `import numpy as np`

## python play movie nokia5110

Python pil example: play movie nokia5110

```python
# -*- coding: utf-8 -*-

import numpy as np
import cv2
import sys
import pcd8544.lcd as lcd
from PIL import Image
import threading
from Queue import Queue
import time

frameQueue_ = Queue()

def readVideo(filename):
    # 動画を開く
    cap = cv2.VideoCapture(filename)

    while(cap.isOpened()):
        try:
            # フレーム読み込み
            ret, frame = cap.read()
            # グレイスケールに変換
            gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        except:
            # 終了
            cap.release()
            cv2.destroyAllWindows()
            quit()

        # リサイズする
        # 先にリサイズしたほうが、二値化後が綺麗
        gray_resized = cv2.resize(gray, (84, 48))
        # ガウス分布を用いたしきい値より二値化
        resize_first = cv2.adaptiveThreshold(gray_resized,255,cv2.ADAPTIVE_THRESH_GAUSSIAN_C,cv2.THRESH_BINARY,11,2)

        # プレビュー表示用
        # cv2.imshow("resize first", resize_first)

        # PCD9544ライブラリがもとめる画像形式に変換
        PIL_data = Image.fromarray(resize_first)
        im = Image.new('1',(84,48))
        im.paste(PIL_data, (0,0) + PIL_data.size)

        frameQueue_.put(im)


 
argvs = sys.argv
argc = len(argvs)

# 動画の指定は引数から行う
if (argc != 2):
    print('Usage: # python2.7 %s filename' % argvs[0])
    quit()

# LCDを初期化
lcd.init()
lcd.backlight(0)    # 1:OFF 0:ON
lcd.set_contrast(0xBF)  # Sparkfunのモジュール（LCD-10168)はこの値が一番見やすい

# 読み込みを別スレッドで行う
t = threading.Thread(target=readVideo, args=(argvs[1],))
t.setDaemon(True)
t.start()

lcd.text("now loading...")

# 500フレーム先読み
while frameQueue_.qsize() < 500:
    time.sleep(1)

# 読み込みが完了し、キューの中身が空になるまで繰り返す
while t.isAlive() or (not frameQueue_.empty()):
    # imageメソッド内でポジション移動等全部やるので余計な処理はいらない
    lcd.image(frameQueue_.get(),reverse=True)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
