---
title: pil example main date (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'main date'


Modules used in program: 
* `import os`

## python main date

Python pil example: main date

```python
from pip._internal import main as pipmain
from datetime import date

try:
    from PIL import Image
except ImportError:
    pipmain(['install', "--upgrade", "Pillow"])
    from PIL import Image
import os


if __name__ == "__main__":
    colorList = ["#ffffff00","#fef585","#80cef3","#6dbe86","#fadac9","#f4b5d0","#fadfda","#f2d4ad","#d5ead8","#b6b6b7","#d2cde6"]
    folders = ["anime","capdoi","cute","nghethuat", "phongcach"]
    codes = []
    f = open("noiluu.txt", "r")
    save_path = f.read()
    today = date.today()
    save_path = os.path.join(save_path,str(today)[5:])
    if not save_path:
        save_path="output"


    if not os.path.exists(save_path):
        os.makedirs(save_path)

    print("Nhap code:")
    while True:
        line = input()
        if line:
            codes.append(line)
        else:
            break
    print("====== Dang in ======")
    for code in codes:
        ma = code.split("_")
        img = Image.new('RGBA', (2423, 1714), color = colorList[int(ma[0])])
        img2 = Image.open(os.path.join("anh",folders[int(ma[4])],str(ma[5])+".png")).convert("RGBA")
        img.paste(img2,(1212,0),img2)
        img2 = Image.open(os.path.join("anh",folders[int(ma[6])],str(ma[7])+".png")).convert("RGBA")
        img.paste(img2,(0,0),img2)
        img.resize((775,549))
        img.save(os.path.join(save_path,str(code) +".png"),"PNG")
        print(":::::")
    print("====== Hoan thanh ======")
    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
