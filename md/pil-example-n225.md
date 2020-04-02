---
title: pil example n225 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'n225'

Functions in program: 
* `def htmlRemover(string):`

Modules used in program: 
* `import time`
* `import sys #モジュールargv`
* `import feedparser`
* `import os`
* `import re`
* `import urllib2`

## python n225

Python pil example: n225

```python
#encoding:UTF-8
#-*- codinf:utf-8 -*-
import urllib2
from BeautifulSoup import BeautifulSoup
import re
import os
from PIL import ImageFont
from PIL import Image
from PIL import ImageDraw
import feedparser
import sys #モジュールargv
import time
argvs = sys.argv #コマンドライン引数を格納したリストの取得
argc = len(argvs) #引数の長さ min:1


def htmlRemover(string):
    p = re.compile(r"<[^>]*?>")
    return p.sub("", string)

##----- KabuData -----##
opener = urllib2.build_opener()
html = opener.open('http://icrus.org/horiba/kabu.html').read()
soup = BeautifulSoup(html)#parse

rapidInc_  = soup.findAll('p',{ "class" : "p1"})
slightInc_ = soup.findAll('p',{ "class" : "p0"})
unchanged_ = soup.findAll('p',{ "class" : "zero"})
slightDec_ = soup.findAll('p',{ "class" : "m0"})
rapidDec_  = soup.findAll('p',{ "class" : "m1"})

rapidInc = []  # 値上がり率大きい
slightInc = [] # 微増
unchanged = [] # 変わらず
slightDec = [] # 微減
rapidDec = []  # 減
isNikkei = [False,False,False,False,False]
##-----株価データのリストを生成 -----##
for items in rapidInc_ :
    rapidInc.append(htmlRemover(str(items)) )
for items in slightInc_ :
    slightInc.append(htmlRemover(str(items)))

for items in unchanged_ :
    unchanged.append(htmlRemover(str(items)))

for items in slightDec_ :
    slightDec.append(htmlRemover(str(items)))

for items in rapidDec_ :
    rapidDec.append(htmlRemover(str(items)))

# p.p1   { color: #E50031; }
# p.p0   { color: #FF0000; }
# p.zero { color: #777777; }
# p.m0   { color: #00FF00; }
# p.m1   { color: #2F4FBA; }

text = ((u"   "),(0,0,0))#dummy data
l = []
#rapidInc
for items in rapidInc:
    l.append( ((u"    ") + unicode(items,"utf-8") ,(229,0,61)) )
#slightInc
for items in slightInc:
    l.append( ((u"    ") + unicode(items,"utf-8") ,(255,0,0)) )
#unchanged
for items in unchanged:
    l.append( ((u"    ") + unicode(items,"utf-8") ,(119,119,119)) )
#slightDec
for items in slightDec:
    l.append( ((u"    ") + unicode(items,"utf-8") ,(0,255,0)) )
#rapidDec
for items in rapidDec:
    l.append( ((u"    ") + unicode(items,"utf-8") ,(47,79,186)) )

#日経平均を先頭に挿入
searchWord = u"日経平均"
counter = 0
for i in l :
    if searchWord in i[0] :
       break
    counter += 1
tmp = l[counter]
l.pop(counter)
l.insert(0,tmp)
#日経平均を先頭に挿入
print(l[0])

text = tuple(l)
#text = ((u"    " + news1,(255,0,255)),
#        (u"    " + news2,(0,255,255)),
#        (u"    " + news3,(0,255,0  ))
#       )
font = ImageFont.truetype("/usr/share/fonts/truetype/takao-mincho/TakaoMincho.ttf", 16)
all_text = ""
for text_color_pair in text:
    t = text_color_pair[0]
    all_text = all_text + t

#print(all_text)
width, ignore = font.getsize(all_text)
#print(width)


im = Image.new("RGB", (width + 30, 16), "black")
draw = ImageDraw.Draw(im)

x = 0;
for text_color_pair in text:
    t = text_color_pair[0]
    c = text_color_pair[1]
    #print("t=" + t + " " + str(c) + " " + str(x))
    draw.text((x, 0), t, c, font=font)
    x = x + font.getsize(t)[0]

im.save("nikkei.ppm")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
