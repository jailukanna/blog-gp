---
title: pil example dem to png (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'dem to png'

Functions in program: 
* `def paste_image(z, p, c, r):`
* `def set_value(dir, rate):`

Modules used in program: 
* `import xml.etree.ElementTree as ET`
* `import numpy as np`
* `import zipfile`
* `import glob`
* `import sys`
* `import re`
* `import os`

## python dem to png

Python pil example: dem to png

```python
# 国土地理院の数値標高データを、一枚絵にするプログラムです。
# 詳細は、下記Qiita記事に(簡単にですが)纏めてますので、ご確認して頂けると幸いです。
# 『Pythonで国土地理院の標高データを画像化してみる』https://qiita.com/artistan/items/99266407702d4152e9d5
# 『Pythonで複数の数値標高データを一枚絵にする』https://qiita.com/artistan/items/a96dd7fb79b6955b09e8

import os
import re
import sys
import glob
import zipfile
import numpy as np
from PIL import Image
import xml.etree.ElementTree as ET

WIDTH = 1125
HEIGHT = 750

# 準備
def set_value(dir, rate):
    global WIDTH
    global HEIGHT
    # ファイル名からメッシュをリスト化し並び替える
    meshList = []
    for f in glob.glob(os.path.join(dir, "*.zip")):
        base = os.path.basename(f)
        meshList.append(base[7:11] + base[12:14])
    meshList.sort()

    # 処理予定データ数
    denominator = len(meshList)
    # 処理済みデータ数
    numerator = 0

    # メッシュリストから最東西南北端のメッシュを調べる
    top = right = meshList[-1]
    bottom = left = meshList[0]
    for meshNumber in meshList:
        str(meshNumber)
        if top[0:2] < meshNumber[0:2] or (top[0:2] <= meshNumber[0:2] and top[4] <= meshNumber[4] and top[4] <= meshNumber[4]):
            top = meshNumber
        if bottom[0:2] > meshNumber[0:2] or (bottom[0:2] >= meshNumber[0:2] and bottom[4] >= meshNumber[4]):
            bottom = meshNumber
        if right[2:4] < meshNumber[2:4] or (right[2:4] <= meshNumber[2:4] and right[5] <= meshNumber[5]):
            right = meshNumber
        if left[2:4] > meshNumber[2:4] or (left[2:4] >= meshNumber[2:4] and left[5] >= meshNumber[5]):
            left = meshNumber
    # 最端のメッシュから、キャンバスサイズを出す
    vartical = (int(top[0:2])-int(bottom[0:2])) * 8 + int(top[4]) - int(bottom[4]) + 1
    horizon = (int(right[2:4])-int(left[2:4])) * 8 + int(right[5]) - int(left[5]) + 1
    maxArea = vartical * horizon
    point = top[0:2] + left[2:4]  + top[4] + left[5]
    # canvas生成
    canvas = Image.new('RGB', (int(WIDTH / rate) * horizon, int(HEIGHT / rate) * vartical), (0, 0, 0))

    # ファイル読み取りと画像化
    for zipfile in glob.glob(os.path.join(dir, "*.zip")):
        paste_image(zipfile, point, canvas, rate)
        numerator += 1
        print('%d / %d' % (numerator, denominator))
    canvas.save('dem.png', 'PNG', quality=100)


# 画像化メソッド
def paste_image(z, p, c, r):
    global WIDTH
    global HEIGHT
    point = p
    canvas = c
    rate = r
    with zipfile.ZipFile(z, "r") as zf:
        for info in zf.infolist():
            inner = info
        with zf.open(inner) as zfile:
            root = ET.fromstring(zfile.read())
            namespace = {
                'xml': 'http://fgd.gsi.go.jp/spec/2008/FGD_GMLSchema',
                'gml': 'http://www.opengis.net/gml/3.2'
            }
            dem = root.find('xml:DEM', namespace)
            # メッシュ番号
            mesh = dem.find('xml:mesh', namespace).text
            # セルの配列数(実際の値は1ずつ追加)
            high = dem.find('./xml:coverage/gml:gridDomain/gml:Grid/gml:limits/gml:GridEnvelope/gml:high', namespace).text.split(' ')
            highX = int(high[0]) + 1
            highY = int(high[1]) + 1

            # 画像サイズ設定(データ数 == ピクセル数)
            dataSize = highX * highY

            # 標高データの配列
            dem_text = dem.find('./xml:coverage/gml:rangeSet/gml:DataBlock/gml:tupleList', namespace).text
            data = re.findall(',(.*)\n', dem_text)
            dataNp = np.empty(dataSize)
            for i in range(len(data)):
                if(data[i] == "-9999.00"):
                    dataNp[i] = 0
                else:
                    dataNp[i] = float(data[i])

            # データの開始座標
            startPoint = dem.find('./xml:coverage/gml:coverageFunction/gml:GridFunction/gml:startPoint', namespace).text.split(' ')
            startPointX = int(startPoint[0])
            startPointY = int(startPoint[1])
            startPosition = startPointY * highX + startPointX

            # データ数が不足している場合(画像の上下に空白が有る場合)
            if(len(dataNp) < dataSize):
                add = []
                # 画像下部のデータが不足している場合
                if(startPosition == 0):
                    for i in range(dataSize - len(dataNp)):
                        add.append(0)
                    dataNp.extend(add)
                # 画像上部及び下部のデータが不足している場合
                else:
                    for i in range(startPosition):
                        add.append(0)
                    dataNp[0:0] = add
                    add = []
                    for i in range(dataSize - len(dataNp) - len(add)):
                        add.append(0)
                    dataNp.extend(add)

            # データの8bit整数化
            dataNp = (dataNp / 15).astype(np.uint8)  # 富士山最高所を255に収める場合、dataNpを15で割る
            data = dataNp.reshape(highY, highX)

            # 数値標高データを画像化
            pilImg = Image.fromarray(np.uint8(data))
            pilImg = pilImg.resize((int(highX / rate), int(highY / rate)), Image.LANCZOS) # NEAREST

            # 貼り付けるメッシュの座標を計算
            targetX = (int(point[0:2]) - int(mesh[0:2])) * 8 + int(point[4]) - int(mesh[4])
            targetY = (int(mesh[2:4]) - int(point[2:4])) * 8 + int(mesh[5]) - int(point[5])

            canvas.paste(pilImg, (targetY * int(WIDTH / rate), targetX * int(HEIGHT / rate)))

# ZIPファイルの入ったディレクトリを入力
DIR = sys.argv[1]
RATE = int(sys.argv[2])

set_value(DIR, RATE)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
