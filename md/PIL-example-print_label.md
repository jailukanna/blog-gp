---
title: PIL example print label (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'print label'

Functions in program: 
* `def do_main():`
* `def read_csv(csv_file):`
* `def draw_label(address_no, address, first_name, last_name, sub_name, sub_sub_name):`
* `def draw_name(draw, text, font_size, posit, img_size):`
* `def draw_text(draw, text, font_size, posit, img_size):`
* `def get_font(size):`

Modules used in program: 
* `import re`
* `import csv`
* `import PIL.ImageFont`
* `import PIL.ImageDraw`
* `import PIL.Image`
* `import os`
* `import sys`
* `import numpy`

## python print label

Python PIL example: print label

```python
#!/usr/bin/env python
# coding:utf-8
"""
Pillowで封筒の宛名書きを行うスクリプト。
CSVを読み込んで、宛名を含んだpngを生成する。

期待するデータは以下のようなフォーマット

<苗字>,<名前>,<郵便番号>,<住所>

"""

import numpy
import sys
import os
import PIL.Image
import PIL.ImageDraw
import PIL.ImageFont
import csv
import re
# 2Lの封筒を想定
canvas_size = (1800, 1200)
# 郵便番号のフォントサイズ
address_no_size = 50
# 住所のフォントサイズ
address_size = 60
# 宛名のフォントサイズ
name_size = 90

left_start_posit = 250
top_start_posit = 300


def get_font(size):
    # フォントファイルを読み込む
    # http://typingart.net/?p=44
    return PIL.ImageFont.truetype("./font/Hannari.otf", size)


def draw_text(draw, text, font_size, posit, img_size):
    """
    住所関連の文字列を描画する
    横幅を超える場合は、サイズを減らしてリトライする
    """
    if font_size < 0:
        raise Exception("Cannnot reduce size")
    draw.font = get_font(font_size)
    txt_size = numpy.array(draw.font.getsize(text))
    print(txt_size)
    # 幅足りる？
    if posit[0] + txt_size[0] > img_size[0] - 30:
        # 再帰
        draw_text(draw, text, font_size - 5 , posit, img_size)

    draw.text(posit, text, (0, 0, 0))


def draw_name(draw, text, font_size, posit, img_size):
    """
    名前関連の文字列を描画する
    住所と違って、名前は横軸をセンタリングしている
    """
    if font_size < 0:
        raise Exception("Cannnot reduce size")
    draw.font = get_font(font_size)
    # 文字サイズの確認
    txt_size = numpy.array(draw.font.getsize(text))
    print(txt_size)
    # センタリング
    pos = (img_size - txt_size) / 2
    # 幅足りる？
    if pos[0] + txt_size[0] > img_size[0]:
        # 足りないならフォントサイズを5pt下げてリトライ
        draw_text(draw, text, font_size - 5, posit, img_size)
    #縦軸は指定を引き継ぐ
    pos[1] = posit[1]
    draw.text(pos, text, (0, 0, 0))
    return pos


def draw_label(address_no, address, first_name, last_name, sub_name, sub_sub_name):
    """
    ラベルを描画する。内部的にはdraw_name/draw_textが行う
    sub_name/sub_sub_nameは連名用
    """
    img = PIL.Image.new("RGB", canvas_size, (0xff, 0xff, 0xff))
    draw = PIL.ImageDraw.Draw(img)
    # はがきのサイズ
    img_size = numpy.array(img.size)
    # 郵便番号
    draw_text(draw, address_no, address_no_size, (left_start_posit, top_start_posit), img_size)
    # 住所、固定で90pt下げる
    draw_text(draw, address, address_size, (left_start_posit, top_start_posit + 90), img_size)
    # 宛名
    name_pos = draw_name(draw, first_name + u"　" + last_name + u" 様", name_size, (left_start_posit, top_start_posit + 250), img_size)
    # 連名がいる場合
    if sub_name:
        # 苗字をぜんぶ全角スペースに置換
        # 苗字は何度もかかず、位置を揃えるため
        spacer = re.sub(".", u"　", first_name)
        # 一人目より120pt下げて描画
        name_pos[1] += 120
        draw_text(draw, spacer + u"　" + sub_name + u" 様", name_size, name_pos, img_size)
    if sub_sub_name:
        # 苗字をぜんぶ全角スペースに置換
        # 苗字は何度もかかず、位置を揃えるため
        spacer = re.sub(".", u"　", first_name)
        name_pos[1] += 120
        draw_name(draw, spacer + u"　" + sub_sub_name + u" 様", name_size, name_pos, img_size)
    return img


def read_csv(csv_file):
    """
    CSVを読込、住所等をJSONにする
    2系のcsvモジュールなのでstrで扱う
    """
    reader = csv.reader(csv_file)
    address_list = []
    for line in reader:
        # 同上チェック
        # print(line[2])
        if line[2] == "同上":
            # 一人前の併記として処理する
            if not address_list[-1]["name3"]:
                address_list[-1]["name3"] = line[1].decode("utf-8")
            else:
                address_list[-1]["name4"] = line[1].decode("utf-8")
            continue

        tmp_dict = {
            "name1": line[0].decode("utf-8"),
            "name2": line[1].decode("utf-8"),
            "name3": None,
            "name4": None,
            "address_no": line[2].decode("utf-8"),
            "address": line[3].decode("utf-8"),
        }
        address_list.append(tmp_dict)
    return address_list


def do_main():
    target_file = sys.argv[1]
    dest_dir = sys.argv[2]
    f = open(target_file)
    address_list = read_csv(f)
    for address in address_list:
        print(address["name1"] + address["name2"])
        img = draw_label(address["address_no"], address["address"], address["name1"], address["name2"], address["name3"], address["name4"])
        img.save(dest_dir+os.sep+address["name1"] + address["name2"]+".png")

if __name__ == "__main__":
    do_main()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
