---
title: PIL example auto recommend ocr helper (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'auto recommend ocr helper'

Functions in program: 
* `def get_question(img_name):`
* `def space_ocr(img,ocr_mode='chs'):`
* `def baidu_ocr(img, app_id = "10661627", app_key = "h5xcL0m4TB8fiiFWoysn7uxt", app_secret = "HGA1cgXzz80douKQUpII7K77TYWSGcfW", api_version=5, timeout=6):`
* `def tesseract_ocr(img,ocr_mode='chi_sim'):`
* `def hanvon_ocr(img_name):`

Modules used in program: 
* `import sys`
* `import thread`
* `import requests`
* `import random`
* `import url_helper`
* `import selenium_helper`
* `import screen_shot_helper`
* `import recommend_helper`
* `import question_helper`
* `import ocr_helper`
* `import jieba_helper`
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

## python auto recommend ocr helper

Python PIL example: auto recommend ocr helper

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
import jieba_helper
import ocr_helper
import question_helper
import recommend_helper
import screen_shot_helper
import selenium_helper
import url_helper
import random
import requests
import thread
from lxml import html
import sys
reload(sys) 
sys.setdefaultencoding('utf8')

def hanvon_ocr(img_name):
#     key='79ce43f8-7c3c-40a8-8bf0-a3aa45384a87'
    key = '3cc4f16c357e4f329dab5e71c810a871'
    key = config.hanvon_ocr['key']
    url='http://api.hanvon.com/rt/ws/v1/ocr/text/recg?key=%s&code=4060d49a-acf8-4897-9f61-5540bda01ed9' % key
#     img=Image.open('1515770523.92.jpg')
#     img=img.convert('L')
#     _w=img.width
#     _h=img.height
#     img=img.resize((_w/3,_h/3),Image.ANTIALIAS)
#     img.save('card_gray.jpg')
    base64img=base64.b64encode(open(img_name,'rb').read())
    data={"uid":'',"lang":'auto',"color":'color',"image":base64img}
    
    headers={"Content-Type":"application/octet-stream"}
    resp=requests.post(url,data=json.dumps(data),headers=headers)
    return json.loads(resp.text)['textResult']
def tesseract_ocr(img,ocr_mode='chi_sim'):
    image=Image.open(img)#����֤��ͼƬ
    # image.load()#����һ��ͼƬ����ֹ�����˴���ʡ��
    # image.show()#����show��չʾͼƬ�������ã���ʡ��
    vcode=pytesseract.image_to_string(image,lang='chi_sim')    
    return vcode
def baidu_ocr(img, app_id = "10661627", app_key = "h5xcL0m4TB8fiiFWoysn7uxt", app_secret = "HGA1cgXzz80douKQUpII7K77TYWSGcfW", api_version=5, timeout=6):
    app_id = config.baidu_ocr['app_id']
    app_key = config.baidu_ocr['app_key']
    app_secret = config.baidu_ocr['app_secret']
    client = AipOcr(appId=app_id, apiKey=app_key, secretKey=app_secret)
    client.setConnectionTimeoutInMillis(timeout * 1000)
    options = {}
    options["language_type"] = "CHN_ENG"
    with open(img, "rb") as fp:
        fp = fp.read()
        result = ''
        if api_version == 1:
            result = client.basicAccurate(fp, options)
        else:
            result = client.basicGeneral(fp, options)
        if "error_code" in result:
            print("baidu api error: ", result["error_msg"])
            return ""
        txt = ''
        try:
            for line in result['words_result']:
                    txt = txt + line['words']+'||'
            return txt
        except:
            return ''
def space_ocr(img,ocr_mode='chs'):
    overlay = True
    api_key = '6c851da45688957'
    language = 'chs'
    filename = img
    payload = {'isOverlayRequired':overlay,
               'apikey':api_key,
               'language':language
               }
    with open(filename,'rb') as f:
        r = requests.post('https://api.ocr.space/parse/image',
                          files={filename:f},
                          data=payload,
                          )
        return json.loads(r.content)['ParsedResults'][0]['ParsedText']
    
def get_question(img_name):
    #调用各种ocr，提取图片中的文字    
    for i in config.ocr_prefer:
        question = ''
        if 'baidu' == i:
            question = baidu_ocr(img_name)
        elif 'space' == i:
            question = space_ocr(img_name)
        if question != '':
            return question
    return ''


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
