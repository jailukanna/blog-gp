---
title: pil example crawl mp (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'crawl mp'

Functions in program: 
* `def crawl_person(link, name):`
* `def crawl_party(link):`
* `def main():`

Modules used in program: 
* `import requests as rq`

## python crawl mp

Python pil example: crawl mp

```python
# Get name and image of mps from sobranie.mk and put them in `PATH`

import requests as rq
from lxml.html import fromstring

PATH = 'target'
base = 'http://www.sobranie.mk/'
start = base+'?ItemID=C5223B907BD9D247A9245C0C30C2E6AE'

def main():
    q = rq.get(start)
    doc = fromstring(q.content)
    links = doc.cssselect("table.DetalnoTabelaTopHome a.WB_SOBRANIE_TocItem[href]")
    for l in links:
        crawl_party(base+ l.attrib['href'])

def crawl_party(link):
    #print('===============')
    q = rq.get(link)
    doc = fromstring(q.content)
    links = doc.cssselect("table.DetalnoTabelaTopHome a.WB_SOBRANIE_ArticleTitle[href]")
    for l in links:
        crawl_person(base + l.attrib['href'], l.text_content() )
        
def crawl_person(link, name):
    name = name.strip()
    if name=='': return
    q = rq.get(link)
    doc = fromstring(q.content)

    imgs = doc.cssselect("table.DetalnoTabelaTop table p img[src]")
    
    if len(imgs) != 1:
        print('Problem with ' + name.encode('utf-8'))
        return
    q = rq.get(base + imgs[0].attrib['src'])
    print(name.encode('utf-8'))
    with open(PATH + '/' +name+'.jpg', 'wb') as f:
        f.write( q.content)
    
if __name__=='__main__':
    main()
       


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
