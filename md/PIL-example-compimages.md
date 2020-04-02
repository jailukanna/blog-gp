---
title: PIL example compimages (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'compimages'

Functions in program: 
* `def minhash(img,otherimg):`
* `def d_hash(img,otherimg):`

Modules used in program: 
* `import imagehash,os`

## python compimages

Python PIL example: compimages

```python
from PIL import Image,ImageFile
import imagehash,os
from glob import glob
#サイズの大きな画像をスキップしない
ImageFile.LOAD_TRUNCATED_IMAGES = True

#2つの画像のハッシュ値の差分を出力
def d_hash(img,otherimg):
    hash = imagehash.phash(Image.open(img))
    other_hash = imagehash.phash(Image.open(otherimg))
    return hash-other_hash
#画像サイズの小さい方を検出する
def minhash(img,otherimg):
    hash_size = Image.open(img).size
    otherhash_size = Image.open(otherimg).size
    if hash_size<otherhash_size: return 0
    else: return 1

#作業フォルダを指定
directory_dir = r'C:\Users\hogehoge\images'
#フォルダリスト、フォルダパスを取得
folder_list = os.listdir(directory_dir)
folder_dir = [os.path.join(directory_dir,i) for i in folder_list if len(os.listdir(os.path.join(directory_dir,i))) >2 ]

#画像リスト、パスを取得
img_list = [os.listdir(i) for i in folder_dir]
img_list_count = [ len( i ) for i in img_list ]
#二重内包表記でフォルダごとの画像リストを作る
img_dir = [ [ os.path.join(dir,list[i]) for i in range(count) if list[i] in 'jpg' or 'png']  for (count,dir,list) in zip(img_list_count, folder_dir, img_list) ]



i = 0
length = len(img_dir)
delete_file = []

#d_hash(),minhash()でフォルダごとの画像を比較
while i < length:
    #進捗状況
    print('i = ',i+'/'+length)
    for j in range(i+1,length):
        #breakへのフラグ
        switch = 0
        for k in img_dir[j]:
            #ハッシュ値の差分が10以下なら同一の画像として認識
            if d_hash(img_dir[i][1],k)<10:
                print(folder_list[i]+' | vs | '+folder_list[j])
                #画像サイズの小さい方のパスをdeleteリストに保存
                if minhash(img_dir[i][1],k) == 0:
                    delete_file.append(folder_dir[i])
                else: delete_file.append(folder_dir[j])
                i += 1
                switch = 1
                break
        if switch != 0:break
    i += 1

#削除したいフォルダパスの表示
print(delete_file)

#続けて削除したい場合
#import shutil
#for i in delete_file:
#   shutil.rmtree(i)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
