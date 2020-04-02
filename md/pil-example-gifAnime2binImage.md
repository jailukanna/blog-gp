---
title: pil example gifAnime2binImage (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'gifAnime2binImage'

Functions in program: 
* `def gif89a_alpha_merge(now_frame, pre_frame):`
* `def cvImageToBinaryString(cvImage):`

Modules used in program: 
* `import sys`
* `import unittest`
* `import time`
* `import cv2`
* `import numpy as np`

## python gifAnime2binImage

Python pil example: gifAnime2binImage

```python
# -*- coding: utf-8 -*-

import numpy as np
import cv2
from PIL import Image, ImageSequence, ImageDraw
import time
import unittest
import sys

# original code : http://playground.arduino.cc/Code/PCD8544

WIDTH = 84
HEIGHT = 48

def cvImageToBinaryString(cvImage):
    row = 0;
    col = 0;
    bit = 0;

    # バイナリの格納先を指定
    ba = np.zeros((HEIGHT/8, WIDTH), dtype=np.uint8)

    # 要素数が正しいかチェック
    assert len(ba) == HEIGHT / 8, "ba height missmuch"
    assert len(ba[0]) == WIDTH, "ba width missmuch"
    # TODO: 全部0か調べる


    PIL_data = Image.fromarray(cvImage).convert('1')
    # PIL_data.show()
    # 0 or 1 の1行バイナリ列に変換
    b = list([0 if x else 1 for x in PIL_data.getdata()])

    binaryString = ""

    for i in range(WIDTH * HEIGHT):
        val = b[i]
        ba[row, col] |= val << bit

        # next col
        col += 1

        # next bit
        if col >= WIDTH:
            col = 0
            bit += 1

        # next data row
        if bit >= 8:
            bit = 0
            for x in range(WIDTH):
                s = "%x" % ba[row, x]
                assert "0x" + s == hex(ba[row, x]), "hex convert failture"
                # Do some formatting
                if len(s) > 2:
                    s = s[-2:]
                while len(s) < 2:
                    s = "0" + s
                binaryString += "0x" + s + ","
            binaryString += "\n"
            row += 1

    # 最後のコロンを削除
    binaryString = binaryString[:-2]

    return binaryString


def gif89a_alpha_merge(now_frame, pre_frame):
    nowf_rgba = now_frame.convert('RGBA')
    pref_rgba = pre_frame.convert('RGBA')
    img = Image.alpha_composite(pref_rgba, nowf_rgba)
    return img


if __name__ == '__main__':

    name = sys.argv[1]

    im = Image.open(name)

    binStr_List = []

    bframe = im.convert('RGB')

    for fn,f in enumerate(ImageSequence.Iterator(im)):  #using the class from the PIL handbook
        # gif89aなら透過色の処理を入れる
        if im.info['version'] == "GIF89a" and fn > 0:
            f = gif89a_alpha_merge(f, bframe)
        PIL_data = f.convert('RGB')
        bframe = PIL_data

        # PILからOpenCVに変換
        OpenCV_RGB = np.asarray(PIL_data)
        # 色をRGB から BGRに変換
        OpenCV_BGR = cv2.cvtColor(OpenCV_RGB, cv2.COLOR_RGB2BGR)

        # グレイスケール化
        gray = cv2.cvtColor(OpenCV_BGR, cv2.COLOR_BGR2GRAY)

        # リサイズする
        # 先にリサイズしたほうが、二値化後が綺麗
        gray_resized = cv2.resize(gray, (84, 48))
        # ガウス分布を用いたしきい値より二値化
        resize_first = cv2.adaptiveThreshold(gray_resized,255,cv2.ADAPTIVE_THRESH_GAUSSIAN_C,cv2.THRESH_BINARY,11,2)

        binStr_List.append( cvImageToBinaryString(resize_first) )

        # 表示
        cv2.imshow('frame', OpenCV_BGR )
        cv2.imshow('binary_mini', resize_first )
        time.sleep(0.1)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cv2.destroyAllWindows()


    # ファイル書き込み
    f = open("gifHeader.h", 'w')

    f.write('#include "Arduino.h"' + "\n")
    f.write('#include <avr/pgmspace.h>' + "\n\n")

    f.write("#define PICTURE_ENTRY " + str(len(binStr_List)) + "\n")
    f.write("#define PICTURE_SIZE " + str(len(binStr_List[0].split(','))) + "\n\n")

    for i,binStr in enumerate(binStr_List):
        f.write("PROGMEM prog_uchar gifAnime_" + str(i) + "[] = { \n")
        # f.write("    { \n      ")
        f.write(binStr)
        # f.write("\n    }")
        f.write(",\n" if i != (len(binStr_List) - 1) else "\n")
        f.write("};\n\n")

    f.write("PROGMEM prog_uchar *gifAnimePointer[] = { \n")
    for i in range(len(binStr_List)):
        f.write("    gifAnime_" + str(i) + ",\n")
    f.write("};\n")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
