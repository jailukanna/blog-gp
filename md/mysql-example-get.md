---
title: mysql example get (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'get'


Modules used in program: 
* `import MySQLdb`
* `import _mysql`
* `import sys`
* `import urllib`

## python get

Python mysql example: get

```python
#coding=utf-8
from pyquery import PyQuery as pq
import urllib
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
from lxml import etree
import _mysql
import MySQLdb

db = MySQLdb.connect(host="localhost",user="root",passwd="",db="wp_test")
db.set_character_set('utf8')

f = open("new.xml")
content = f.read()
content = content.decode("utf-8")

d = pq(content)
entrys = d("entry")
categ = {
    "日常 生活" : 1,
    "链接表" : 2,
    "照片" : 5,
    "好习惯，坏习惯" : 9,
    "瑜伽 运动" : 11,
    "规矩" : 12,
    "关于you" : 14,
    "家人" : 15,
    "学习" : 21,
    "学习计划" : 22,
    "生病的日子" : 23,
    "读书笔记" : 19
}
#print(entrys[1].find('content')[0].text)
for entry in entrys:
    content = entry.find('content')
    title = entry.find('title').text
    published = entry.find('published').text
    category = entry.find('category')
    category = category.attrib['term'].encode('utf-8')
    try:
        categId = categ[category]
    except:
        categId = 4
        pass
    c=db.cursor()
    max_price=5
    content = ''.join([etree.tostring(child,encoding='unicode') for child in content.iterdescendants()])
    content = MySQLdb.escape_string(content)
    sql = "INSERT INTO `wp_test`.`wp_posts` (`ID`, `post_author`, `post_date`, `post_date_gmt`, `post_content`, `post_title`, `post_excerpt`, `post_status`, `comment_status`, `ping_status`, `post_password`, `post_name`, `to_ping`, `pinged`, `post_modified`, `post_modified_gmt`, `post_content_filtered`, `post_parent`, `guid`, `menu_order`, `post_type`, `post_mime_type`, `comment_count`) VALUES (NULL, '1', '"+published+"', '"+published+"', '"+content+"', '"+title+"', '', 'publish', 'open', 'open', '', '', '', '', '0000-00-00 00:00:00', '0000-00-00 00:00:00', '', '0', '', '0', 'post', '', '0');"
    c.execute(sql)
    id = db.insert_id()
    sql = "INSERT INTO `wp_test`.`wp_term_relationships` (`object_id`, `term_taxonomy_id`, `term_order`) VALUES ('"+str(id)+"', '"+str(categId)+"', '0');"
    c.execute(sql)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
