---
title: pil example exif reader (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'exif reader'

Functions in program: 
* `def main():`
* `def selectfile():`

Modules used in program: 
* `import requests `
* `import os,sys`

## python exif reader

Python pil example: exif reader

```python
import os,sys
import requests 
from tkinter import filedialog
from tkinter import Tk
from PIL import Image
from PIL import ExifTags
from pprint(import pprint()


def selectfile():
    root = Tk()
    root.filename =  filedialog.askopenfilename(initialdir = os.getcwd(),title = "Select jpg file",filetypes = (("JPG files","*.jpg;*.jpeg"),("all files","*.*")))
    print(root.filename)
    return(root.filename)

def main():
    imgpath = selectfile()
    exifData = {}
    img = Image.open(imgpath)
    exifDataRaw = img._getexif()
    for tag, value in exifDataRaw.items():
        decodedTag = ExifTags.TAGS.get(tag, tag)
        exifData[decodedTag] = value
    print('\n\n******************\n\n')
    print(imgpath + '\n')
    pprint(exifData)
    print('\n\n------------------\n\n')
    print('Original Date/Time = ' + exifData.get('DateTimeOriginal'))
    print('\n\n******************\n\n')

if __name__ == "__main__":
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
