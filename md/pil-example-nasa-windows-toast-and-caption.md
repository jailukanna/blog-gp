---
title: pil example nasa-windows-toast-and-caption (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'nasa-windows-toast-and-caption'

Functions in program: 
* `def drawTextWithOutline(text, x, y):`

Modules used in program: 
* `import ctypes`
* `import requests`
* `import os`

## python nasa-windows-toast-and-caption

Python pil example: nasa-windows-toast-and-caption

```python
import os
import requests
import ctypes
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw 

from win10toast import ToastNotifier
url = "https://api.nasa.gov/planetary/apod?api_key="
os.environ['NO_PROXY'] = 'nasa.gov'
r = requests.get(url)

toaster = ToastNotifier()

def drawTextWithOutline(text, x, y):
    draw.text((x-2, y-2), text,(0,0,0),font=font)
    draw.text((x+2, y-2), text,(0,0,0),font=font)
    draw.text((x+2, y+2), text,(0,0,0),font=font)
    draw.text((x-2, y+2), text,(0,0,0),font=font)
    draw.text((x, y), text, (255,255,255), font=font)
    return

if r:
    APOD = r.json()['hdurl']
    title = r.json()['title']
    detail = r.json()['explanation']
    pic = requests.get(APOD, allow_redirects=True)
    if  "jpg" not in APOD:
        print("No image")
    else:
        open('C:\\Users\\vincent.willcox\\Pictures\\APOD.jpg', 'wb').write(pic.content)
        img = Image.open('C:\\Users\\vincent.willcox\\Pictures\\APOD.jpg')
        draw = ImageDraw.Draw(img)
        font = ImageFont.truetype('GrabsteinHandSchrift.otf', 100)
        font2 = ImageFont.truetype('GrabsteinHandSchrift.otf', 10)
        w, h = draw.textsize(title, font)
        drawTextWithOutline(title, img.width/2 - w/2, img.height-100)
        img.save('C:\\Users\\vincent.willcox\\Pictures\\APOD.jpg')
        ctypes.windll.user32.SystemParametersInfoW(20, 0, "C:\\Users\\vincent.willcox\\Pictures\\APOD.jpg" , 0)
        toaster.show_toast(title, detail, icon_path=None, duration=10, threaded=True)
        print("DONE")
else:
    print("Error")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
