---
title: PIL example server-001 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'server-001'

Functions in program: 
* `def index(name):`
* `def index(name):`

Modules used in program: 
* `import pytesseract`

## python server-001

Python PIL example: server-001

```python
from bottle import route, run, template, get, post, request
import pytesseract
from PIL import Image

@route('/dev/<name>')
def index(name):
    Image.open('images/%s.png' %(name)).save('/tmp/%s.jpg' %(name))
    im=Image.open('/tmp/'+ name +'.jpg')
    out=pytesseract.image_to_string(im,lang='hin')
    return template('<b>Meaning of {{name}}:</b>!<br><img src="/tmp/%s.jpg"><br/>%s' %(out,name), name=name)

@route('/eng/<name>')
def index(name):
    Image.open('images/%s.png' %(name)).save('/tmp/%s.jpg' %(name))
#    os.system('convert images/%s.png /tmp/%s.jpg' %(name, name))
    im=Image.open('/tmp/'+ name +'.jpg')
    out=pytesseract.image_to_string(im)
    return template('<b>Meaning of {{name}}:</b>!<br/>%s' %out, name=name)

# print(pytesseract.image_to_string(Image.open('test-european.jpg'), lang='fra'))
run(host='0.0.0.0', port=8080)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
