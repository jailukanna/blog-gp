---
title: PIL example auto recommend img compress helper (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'auto recommend img compress helper'

Functions in program: 
* `def img_zip(path,filename1,filename2):`

Modules used in program: 
* `import config`
* `import time`
* `import cv2`
* `import sys`

## python auto recommend img compress helper

Python PIL example: auto recommend img compress helper

```python
#encoding=utf-8
#############设置编码################
import sys
reload(sys)
sys.setdefaultencoding('utf-8')

############导入计算机视觉库opencv和图像处理库PIL####################
from PIL import Image
from PIL import ImageEnhance
from PIL import ImageFilter
import cv2
import time
import config
time1 = time.time()

########################自定义图像压缩函数############################
def img_zip(path,filename1,filename2):
    image = cv2.imread(path+filename1)
    res = cv2.resize(image, config.screen_shot_size, interpolation=cv2.INTER_AREA)
    cv2.imwrite(path+filename2, res)
    imgE = Image.open(path+filename2)
    imgEH = ImageEnhance.Contrast(imgE)
    img1 = imgEH.enhance(1)
    gray1 = img1.convert("L")
    gary2 = gray1.filter(ImageFilter.DETAIL)
#     gary3 = gary2.point(lambda i: i * 0.9)
#     gary3.save(path+filename2)
    gary2.save(path+filename2)
#     gray1.save(path+filename2)

################################主函数##################################
if __name__ == '__main__':
    path="E:/neon_workspace/auto_recommend/"
    filename1="1515924735.05.png"
    filename2="compress.jpg"
    img_zip(path,filename1,filename2)
    time2 = time.time()
    print('总共耗时：' + str(time2 - time1) + 's')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
