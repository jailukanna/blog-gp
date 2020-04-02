---
title: PIL example finder (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'finder'

Functions in program: 
* `def main():`
* `def pilToCv2(im):`
* `def screenGrab():`
* `def click(x,y):`

Modules used in program: 
* `import win32api, win32con`
* `import numpy as np`
* `import cv2`

## python finder

Python PIL example: finder

```python
from time import sleep
import cv2
import numpy as np
from matplotlib import pyplot as plt
from PIL import ImageGrab
import win32api, win32con

def click(x,y):
    win32api.SetCursorPos((x,y))
    win32api.mouse_event(win32con.MOUSEEVENTF_LEFTDOWN,x,y,0,0)
    win32api.mouse_event(win32con.MOUSEEVENTF_LEFTUP,x,y,0,0)

def screenGrab():
    return ImageGrab.grab()

def pilToCv2(im):
    return np.asarray(im)[:,:,::-1].copy()

def main():
    img_rgb = pilToCv2(screenGrab())
    img_gray = cv2.cvtColor(img_rgb, cv2.COLOR_BGR2GRAY)
    template = cv2.imread('mario_coin.png',0)
    w, h = template.shape[::-1]
    res = cv2.matchTemplate(img_gray,template,cv2.TM_CCOEFF_NORMED)
    threshold = 0.8
    loc = np.where( res >= threshold)
    for pt in zip(*loc[::-1]):
        center = (int(((pt[0] * 2 + w) / 2)),int(((pt[1] * 2 + h) / 2)))
        click(center[0],center[1])
        sleep(0.05)

if __name__ == '__main__':
    while True:
        main()
        sleep(1)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
