---
title: pil example auto recommend jieba helper (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'auto recommend jieba helper'

Functions in program: 
* `def jieba_parse(question,md=config.jieba_md):`

Modules used in program: 
* `import sys`
* `import thread`
* `import requests`
* `import random`
* `import jieba.analyse`
* `import jieba`
* `import operator`
* `import win32gui, win32ui, win32con`
* `import threading`
* `import config`
* `import url_helper`
* `import selenium_helper`
* `import screen_shot_helper`
* `import recommend_helper`
* `import question_helper`
* `import ocr_helper`
* `import jieba_helper`
* `import urllib,urllib2`
* `import re`
* `import time`
* `import pytesseract`
* `import os,base64,json`
* `import colorsys`

## python auto recommend jieba helper

Python pil example: auto recommend jieba helper

```python
#encoding=utf-8
from PIL import Image
import colorsys
import os,base64,json
import pytesseract
import time
import re
import urllib,urllib2
import jieba_helper
import ocr_helper
import question_helper
import recommend_helper
import screen_shot_helper
import selenium_helper
import url_helper
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
import jieba.analyse
import random
import requests
import thread
from lxml import html
import sys
reload(sys) 
sys.setdefaultencoding('utf8')

def jieba_parse(question,md=config.jieba_md):
    result = ''
    pattern1 = re.compile(r'(《[\w\W]*》)')
    fixed_word_ls = re.findall(pattern1, question)
    if len(fixed_word_ls)>0: 
        for i in fixed_word_ls:
            question = " ".join(question.split(i))
            result = result+i+" "
    if md == 0:
        seg_list = jieba.cut_for_search(question)  # 搜索引擎模式    
    if md == 1:
        seg_list = jieba.cut(question, cut_all=True)
    if md == 2:
        seg_list = jieba.cut(question, cut_all=False)
    if md == 3:
        seg_list = jieba.analyse.extract_tags(question, topK=config.search_keyword_num)
    for i in seg_list:
        if i in config.jieba_filter_ls:
            continue
        result = result + i +' '    
    return result

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
