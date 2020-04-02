---
title: PIL example parse imgur (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'parse imgur'

Functions in program: 
* `def main():`

Modules used in program: 
* `import requests as rq`

## python parse imgur

Python PIL example: parse imgur

```python
# Get large thumbnails from imgur album with album_id `album_id`
 
import requests as rq
from lxml.html import fromstring

album_id = "qPOcT"

def main():
    q = rq.get("http://api.imgur.com/2/album/"+album_id)
    doc = fromstring(q.content)
    for l in doc.cssselect("large_thumbnail"):
        print(l.text_content())
        
    
if __name__=='__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
