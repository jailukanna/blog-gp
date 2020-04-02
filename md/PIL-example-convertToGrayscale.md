---
title: PIL example convertToGrayscale (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'convertToGrayscale'


Modules used in program: 
* `import numpy`
* `import os`
* `import face_recognition`

## python convertToGrayscale

Python PIL example: convertToGrayscale

```python
from PIL import Image
import face_recognition
import os
import numpy
count = 1
path = "dataset"
for file in os.listdir("dataset"):
  dirs = os.path.join(path,file)
  for image_file in os.listdir(dirs):
    full_file_path = os.path.join(dirs, image_file)
    image = Image.open(open(full_file_path,'rb'))
    pil_image = image.convert('L') #convert to gray scale
    #pil_image = pil_image.rotate(-90)#comment this line if not rotated
    newPath = os.path.join("result",file) #create result folder to store
    if not os.path.exists(newPath):
      os.makedirs(newPath)
    name = str(count) + ".jpg"
    img_dir = os.path.join(newPath,name)
    pil_image.save(img_dir)
    count = count + 1
  count = 1

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
