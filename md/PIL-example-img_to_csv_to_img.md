---
title: PIL example img to csv to img (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'img to csv to img'


Modules used in program: 
* `import csv`
* `import numpy as np`
* `import PIL.ImageOps`
* `import matplotlib.pyplot as plt`

## python img to csv to img

Python PIL example: img to csv to img

```python
import matplotlib.pyplot as plt
from PIL import Image
from PIL import ImageFilter
import PIL.ImageOps
import numpy as np
import csv

# 이미지 파일 그레이 스케일 로드 후 리사이즈
im = Image.open('./FACE/face1.jpg').convert('L')
im = im.resize((98, 98))

# 98*98 크기의 이미지를 1차원 배열로 변경
imgarr = np.array(im)
imgarr = imgarr.reshape((1, 9604))

# 배열을 CSV 파일로 저장
csvfile = open("output.csv", 'w', newline='')
csvwriter = csv.writer(csvfile)
for row in imgarr:
    csvwriter.writerow(row)
csvfile.close()

# CSV 파일을 읽은 뒤 이미지로 변환
bufarr = np.loadtxt(fname='output.csv', delimiter=',', dtype= np.uint8)
bufarr = bufarr.reshape(98, 98)
img = Image.fromarray(bufarr, 'L')
#img.save('buf.png')
#img = PIL.ImageOps.invert(img)  #이미지 반전

# matplotlib.pyplot 을 통하여 디스플레이
plt.imshow(img, cmap='Greys', interpolation='nearest')
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
