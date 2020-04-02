---
title: pil example auto recommend config (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'auto recommend config'


## python auto recommend config

Python pil example: auto recommend config

```python
#encoding=utf-8
baidu_ocr = {
    'app_id':'10684752',
    'app_key':'KbitiVi5pyGBebM0Ujcm3zRl',
    'app_secret':'js72wMVICk6sNW1LDubxsoGpVF7yoLa6'
    }
hanvon_ocr = {
    
    }
path="E:/neon_workspace/auto_recommend/"
ref_url = [
    'http://www.baidu.com/s?wd=',
    'http://zhidao.baidu.com/search?word=',
    ]
ocr_prefer = ('baidu','space','hanvon','tesseract')
search_keyword_num = 8
screen_shot_position = (38,173)
screen_shot_size = (505,370)
FALSE = (
    "不是",
    "是错",
    "不正确",
    "不属于",
    "没有",
    "不对的是",
)
word_escape = {
    "我国":"中国",
    "哪国":"国家 国籍",
    }

params_key = {
    'baidu':'wd',
    'baike':'word'
    }
jieba_md = 3
filter_ls = [
    ("“", "\""), 
    ("”", "\""), 
    ("？", " "),
    ("下列"," "),
    ("。"," "),
    ("?"," "),
    ("的是"," "),
    ("闻名于"," "),
    (":"," "),
    (","," "),
    ("所说的"," "),
    ("这个"," "),
    ("一下"," "),
    ("在哪儿"," "),
    ("是什么"," "),
    ("关于"," "),
    ("．"," ")
    ]
jieba_filter_ls = [
    "\"",
    "'"
    ":",
    ",",
    "．",
    " 、",
    "?",
    "“",
    "”",
    "？",
    ",",
    "。",
    "的",
    "是",
    "由",
    "种",
    "哪",
    "中",
    "过",
    "有",
    "叫",
    "下列",
    "以下",
    "法中",
    "关于",
    "哪种",
    "哪个",
    "什么",
    "这个",
    "之中",
    "一下",
    "我们",
    "的是",
    "正常",
    "来说",
    "闻名于",
    "一个程",
    "所说的",
    "在哪儿",
    "是什么",
    ]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
