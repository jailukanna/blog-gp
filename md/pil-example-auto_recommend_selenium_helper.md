---
title: pil example auto recommend selenium helper (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'auto recommend selenium helper'

Functions in program: 
* `def browser_init(url="https://www.baidu.com"):`
* `def baidu_search(driver,search_word):`

Modules used in program: 
* `import sys`
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
* `import url_helper`
* `import selenium_helper`
* `import screen_shot_helper`
* `import recommend_helper`
* `import question_helper`
* `import ocr_helper`
* `import jieba_helper`
* `import time`
* `import pytesseract`
* `import os,base64,json`
* `import colorsys`

## python auto recommend selenium helper

Python pil example: auto recommend selenium helper

```python
#encoding=utf-8
from PIL import Image
import colorsys
import os,base64,json
import pytesseract
import time
import jieba_helper
import ocr_helper
import question_helper
import recommend_helper
import screen_shot_helper
import selenium_helper
import url_helper
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
from lxml import html
import sys
reload(sys) 
sys.setdefaultencoding('utf8')

def baidu_search(driver,search_word):
    WebDriverWait(driver, 20, 0.5).until(lambda x: x.find_element_by_id('kw'))
    #  输入要爬的关键词
    input_area = driver.find_element_by_id('kw')
    input_area.clear()
    input_area.send_keys(search_word.decode())
    submit_btn = driver.find_element_by_id('su')
    submit_btn.click()

def browser_init(url="https://www.baidu.com"):
    #  启动浏览器
    driver = webdriver.Firefox()
    # driver = webdriver.Chrome(executable_path="C:/Program Files (x86)/Google/Chrome/Application/chromedriver.exe")  
    driver.get(url)
    return driver

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
