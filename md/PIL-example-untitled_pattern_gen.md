---
title: PIL example untitled pattern gen (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'untitled pattern gen'

Functions in program: 
* `def interpolate(color_arr, mul):`

Modules used in program: 
* `import numpy as np`

## python untitled pattern gen

Python PIL example: untitled pattern gen

```python
"""
title       : パターン画像生成スクリプト
project     : background pattern generator
filename    : pattern_gen_test.py

description : パターン画像作成
              基本機能のみ　色や数はコード内で直接指定
              ver   date  log
              1.1   0802  (R,G,B)形式で示したパターンの生成に成功
              1.2   0803  パターン間の補間関数作成

date        : 2017/08/02
version     : 1.1

reference   : https://librabuch.jp/blog/2013/05/python_pillow_pil/

"""
from PIL import Image, ImageFont, ImageDraw
import numpy as np

def interpolate(color_arr, mul):
    pat_num = int(color_arr[:, 1].size) #初期色数
    print("pat_num: "+ str(pat_num))
    ret_arr = np.empty((0,3), int)
    #for i in range(pat_num):
    #    for j in range(mul):
    #        col_tmp = (int((j+1)*color_arr[i,0]/mul), int((j+1)*color_arr[i,1]/mul), int((j+1)*color_arr[i,2]/mul))
    #        print(col_tmp)
    #        ret_arr = np.append(ret_arr, np.array([col_tmp]), axis=0)
    #        print("ret_array: " +str(ret_arr))

    for i in range(pat_num-1):
        dif = (color_arr[i+1] - color_arr[i] )/mul
        print(dif)
        for j in range(mul):
            col_tmp = (int(color_arr[i,0] + j*dif[0]), int(color_arr[i,1] + j*dif[1]),int(color_arr[i,2] + j*dif[2]))
            print(col_tmp)
            ret_arr = np.append(ret_arr, np.array([col_tmp]), axis=0)
            print("ret_array: " + str(ret_arr))

    print("ret_array: " + str(ret_arr.size))
    return ret_arr

if __name__ == '__main__':
    #画像サイズ, パターン数を規定（割り切れるように！）
    img_size = (1920,1080)
    gen_code = np.array([(106,106,158), (67,170,166), (103,186,218)])

    pat_code = interpolate(gen_code, 7)
    print(pat_code)
    pat_num = int(pat_code[:,1].size)
    pat_size = (int(img_size[0]/pat_num), int(img_size[1]))

    #マージに利用する下地を作成(FULLHDサイズ)
    canvas = Image.new('RGB', img_size, (255,255,255))
    #draw = ImageDraw.Draw(canvas)

    #パターンの各要素を作成
    patt = np.zeros(pat_num)
    print(pat_size)
    for x in range(0,int(pat_num)):
        print(pat_code[x])
        cctup = (pat_code[x,0], pat_code[x,1], pat_code[x,2])
        #print(cctup)
        patt_tmp = Image.new('RGB', pat_size, cctup)
        pos = int(x * pat_size[0])
        print(pos)
        canvas.paste(patt_tmp, (pos, 0))

    #下地にパターンの要素を貼り付け
    #canvas.paste(pat_red,(0,0))
    #canvas.paste(pat_green,(500,0))
    #for x in range[pat_num]:
    #    pos = int(x*img_size[1]/pat_num)
    #    canvas.paste(patt[x], (pos,0))

    #font = ImageFont.truetype('C:\Windows\Fonts\ipaexg.ttf', 80)

    # 指定の座標にテキスト書き込み 書き込み座標, テキスト, フォント, 文字色
    # fill には16進のカラーコード
    #draw.text((960,500), 'Firenze', font=font, fill='#FFFFFF')

    #パターン画像生成
    canvas.save('triad.jpg', 'JPEG', quality = 100, optimize = True)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
