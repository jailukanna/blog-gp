---
title: matplotlib example seitaipen (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'seitaipen'

Functions in program: 
* `def plot1data(data):`
* `def plot3data(data1, data2, data3):`
* `def calc_integral(T):`
* `def openfile(filename):`

Modules used in program: 
* `import matplotlib`
* `import matplotlib.pyplot as plt`
* `import math`

## python seitaipen

Python matplotlib example: seitaipen

```python
# python を用いて9つのデータを一気に処理するプログラム

# 実行すると，L2ノルムでの積分の値が次々に表示され，

# 最後に図が表示される。

# ところどころコメントアウトを外すとグラフが保存されたりいろいろする。

import math

import matplotlib.pyplot as plt

import matplotlib

# ubuntu（グラフに日本語を表示させたい！）

# from matplotlib.font_manager import FontProperties

# sfont_path = '/usr/share/fonts/truetype/takao-gothic/TakaoExGothic.ttf' # 適宜インストールされているフォントを選ぶ

# font_prop = FontProperties(fname=font_path)

# matplotlib.rcParams['font.family'] = font_prop.get_name()

# mac（グラフに日本語を表示させたい！）

font = {'family':'YuGothic'} # 適宜インストールされているフォントを選ぶ

matplotlib.rc('font',**font)

def openfile(filename):

  T = []

  with open(filename, "r") as f:

    for line in f:

      T.append(line[:-2].split(' ')[:-1])

  return T[1:]

def calc_integral(T):

  integral = 0

  n = len(data)

  for i in range(0,n):

    integral += float(data[i][1]) ** 2

  return math.sqrt(integral)

def plot3data(data1, data2, data3):

  plt.figure()

  name = [1, 2, 3]

  hardnesslist = ['HB', '2B', '6B']

  plt.plot(name, data1[1:], label = data1[0])

  plt.plot(name, data2[1:], label = data2[0])

  plt.plot(name, data3[1:], label = data3[0]) 

  title = 'hoge' # プロットの中に表示されるタイトル（例：積分値） 

  plt.title(title)

  plt.legend()

  plt.xticks(name, hardnesslist)

  plt.xlabel('x')

  plt.ylabel('y')

  figtitle = 'hogehoge' # 保存するファイルの名前（例：dataplot.png）

#   plt.savefig(figtitle) # figtitleに入れた名前でプロット画像を保存

  plt.show()


def plot1data(data):

  plt.figure()

  plt.scatter(hardnesslist, data1[1:], label = data1[0])

  title = 'hoge'

  plt.title(title)

  plt.legend()

  plt.xlabel('x')

  plt.ylabel('y')

  figtitle = 'hogehoge'

#   plt.savefig(figtitle) # figtitleに入れた名前でプロット画像を保存

  plt.show()

namelist = ['m', 'n', 'u']

hardnesslist = ['hb', '2b', '6b']

plotdata = []

subplotdata = []

for name in namelist:

  for hardness in hardnesslist:

    integral = 0

    for i in range(1,4):

      filetitle = name + name + "-" + hardness + "-" + str(i) + ".dat"

      data = openfile(filetitle)

      integral += calc_integral(data)

    print(name, hardness, integral/3) # 試しに出力

    subplotdata.append(integral/3)

for i in range(0,3):

  A = [namelist[i]]

  for j in range(0,3):

    A.append(subplotdata[3*i+j])

  plotdata.append(A)

plot3data(plotdata[0], plotdata[1], plotdata[2])

### おまけ ###

# texファイル用の表を作成

# table_tex = ""

# for i in range(0,3):

#   for j in range(0,4):

#     if j == 0: table_tex += str(plotdata[i][j])

#     if j != 0: table_tex += " & " + str(round(plotdata[i][j], 3)) # 後ろの3は四捨五入する桁数

#   table_tex += " \\\\"

#   if i == 2: table_tex += " \\hline"

#   if i != 2: table_tex += '\n'

# print(table_tex)

メッセージ入力

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
