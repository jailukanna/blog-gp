---
title: PIL example auto recommend screen shot helper (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'auto recommend screen shot helper'

Functions in program: 
* `def isQuestion(img_name):`
* `def get_dominant_color(image):`
* `def window_capture(filename,position=(0,0), size=(1920,1080)):`

Modules used in program: 
* `import sys`
* `import url_helper`
* `import selenium_helper`
* `import screen_shot_helper`
* `import recommend_helper`
* `import question_helper`
* `import ocr_helper`
* `import jieba_helper`
* `import thread`
* `import requests`
* `import random`
* `import jieba`
* `import operator`
* `import win32gui, win32ui, win32con`
* `import threading`
* `import config`
* `import urllib,urllib2`
* `import re`
* `import time`
* `import pytesseract`
* `import os,base64,json`
* `import colorsys`

## python auto recommend screen shot helper

Python PIL example: auto recommend screen shot helper

```python
#encoding=utf-8
from PIL import Image
import colorsys
import os,base64,json
import pytesseract
import time
import re
import urllib,urllib2
import config
import threading
import win32gui, win32ui, win32con
# import win32api
from selenium import webdriver
# import traceback
from selenium.webdriver.support.wait import WebDriverWait
# import urllib, urllib2
# import StringIO 
from aip import AipOcr
import operator
import jieba
import random
import requests
import thread
import jieba_helper
import ocr_helper
import question_helper
import recommend_helper
import screen_shot_helper
import selenium_helper
import url_helper
from lxml import html
import sys
reload(sys) 
sys.setdefaultencoding('utf8')


def window_capture(filename,position=(0,0), size=(1920,1080)):
    try:
        hwnd = 0
        hwndDC = win32gui.GetWindowDC(hwnd)
        mfcDC = win32ui.CreateDCFromHandle(hwndDC)
        saveDC = mfcDC.CreateCompatibleDC()
        saveBitMap = win32ui.CreateBitmap()
        saveBitMap.CreateCompatibleBitmap(mfcDC, size[0], size[1])
        saveDC.SelectObject(saveBitMap)
        saveDC.BitBlt((0, 0), size, mfcDC, position, win32con.SRCCOPY)
        saveBitMap.SaveBitmapFile(saveDC, filename)
    except:
        print('截图异常')
    
def get_dominant_color(image):
    #颜色模式转换，以便输出rgb颜色值
    image = image.convert('RGBA')
    #生成缩略图，减少计算量，减小cpu压力
    image.thumbnail((200, 200))
    max_score = 0#原来的代码此处为None
    dominant_color = 0#原来的代码此处为None，但运行出错，改为0以后 运行成功，原因在于在下面的 score > max_score的比较中，max_score的初始格式不定
    for count, (r, g, b, a) in image.getcolors(image.size[0] * image.size[1]):
        # 跳过纯黑色
        #         if a == 0:
        #             continue
        #         
        saturation = colorsys.rgb_to_hsv(r / 255.0, g / 255.0, b / 255.0)[1]
       
        y = min(abs(r * 2104 + g * 4130 + b * 802 + 4096 + 131072) >> 13, 235)
       
        y = (y - 16.0) / (235 - 16)
        
        # 忽略高亮色
        #         if y > 0.9:
        #             continue
        
        # Calculate the score, preferring highly saturated colors.
        # Add 0.1 to the saturation so we don't completely ignore grayscale
        # colors by multiplying the count by zero, but still give them a low
        # weight.
        score = (saturation + 0.1) * count
        
        if score > max_score:
            max_score = score
            dominant_color = (r, g, b)
    
    return dominant_color    
def isQuestion(img_name):
    domain_color = get_dominant_color(Image.open(img_name))
    if domain_color[0]<210 or domain_color[1]<210 or domain_color[2]<210:
#         print('---主持人废话，不处理',)
#         print(domain_color)
        os.remove(img_name)
        return False
    print('***************')
    print('**    题目       **')
    print('***************')
    return True

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
