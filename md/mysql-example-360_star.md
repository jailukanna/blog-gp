---
title: mysql example 360 star (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example '360 star'

Functions in program: 
* `def write2db(conn, names_list, links_list, field, area, sex):`
* `def run():`
* `def make_query(base_url, arg1, arg2, arg3):`

Modules used in program: 
* `import sys`
* `import mysql.connector`
* `import pinyin`
* `import requests`

## python 360 star

Python mysql example: 360 star

```python
#!/usr/bin/python
# -*-coding: utf-8 -*-

import requests
from bs4 import BeautifulSoup
import pinyin
import mysql.connector
import sys

count_index = 0
def make_query(base_url, arg1, arg2, arg3):
    '''This function construct the real request address and returns the result'''
    addInfo = 'field:%s|area:%s|sex:%s' % (arg1, arg2, arg3)
    payload = {
        'query': '明星大全',
        'url': 'link',
        'type': 'star_card',
        'rank': '1',
        'addInfo': addInfo,
        'd': 'pc',
        'jsonpTest': 'on',
        '_test': '1',
        '_debug': '0',
        'src': 'onebox',
        'tpl': '1'
    }
    header = {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36'}
    r = requests.get(base_url, params=payload)
    return r

def run():
    global count_index
    base_url = 'http://open.onebox.haosou.com/dataApi'
    field_list = ['明星', '歌手', '演员', '模特', '主持人', '导演']
    area_list = ['内地', '香港', '台湾', '日本', '韩国']
    sex_list = ['男', '女']
    conn = mysql.connector.connect(unix_socket='/run/mysqld/mysqld.sock', user='username', password='password', database='database', charset='utf8')
    for field in field_list:
        for area in area_list:
            for sex in sex_list:
                r = make_query(base_url, field, area, sex)
                if r.status_code == 200:
                    html_text = r.text
                    soup = BeautifulSoup(html_text, from_encoding='utf-8')
                    link_div = soup.find('div', {'class': 'mh-datas'})
                    links = link_div.text.strip(',')
                    links_list = links.split(',')
                    name_div = soup.find('div', {'class': 'mh-names'})
                    names = name_div.text.strip(',')
                    names_list = names.split(',')
                    write2db(conn, names_list, links_list, field, area, sex)
                    print(count_index)
                    count_index += 1
    conn.close()

def write2db(conn, names_list, links_list, field, area, sex):
    cur = conn.cursor()
    name_len = len(names_list)
    link_len = len(links_list)
    if name_len <= link_len:
        l = name_len
    else:
        l = link_len
    for i in range(l):
        try:
            name = names_list[i]
            name_py = pinyin.get(name)
            link = links_list[i]
            sql = "insert into 360_star(name, name_py, pic_link, field, area, sex) values(%s, %s, %s, %s, %s, %s)"
            cur.execute(sql, (name, name_py, link,  field, area, sex))
        except UnicodeEncodeError:
            print(name)
            continue
    conn.commit()
    cur.close()

run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
