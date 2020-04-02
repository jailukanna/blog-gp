---
title: pil example ImageReducer (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ImageReducer'

Functions in program: 
* `def main():`

Modules used in program: 
* `import os`

## python ImageReducer

Python pil example: ImageReducer

```python
#PIL (python image library) make sure you have PIL installed, otherwise install it from here my typing commnad ->pip install Pillow
#if you have PIL already you are good to go.

import os
from PIL import Image
def main():
    select_folder='C:\\Users\\atulc\\Downloads\\The Best Cars HD Wallpapers'    #my first folder or source folder
    dest_folder='D:\\my walls\\cars'                                            #my destination folder
    for image_path in os.listdir(select_folder):
        print(image_path)
        input_path=os.path.join(select_folder,image_path)
        img=Image.open(input_path)
        print(input_path)
        new_image=img.resize(img.size,Image.ANTIALIAS)
        dest_path=os.path.join(dest_folder,'cmpr'+image_path)                   #cmpr represent the compressed image you can chnage it to other name
        new_image.save(dest_path)

if __name__=='__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
