---
title: PIL example manipulate (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'manipulate'


Modules used in program: 
* `import PIL`
* `import fitz`

## python manipulate

Python PIL example: manipulate

```python
# coding: utf-8
import fitz
doc = fitz.open('outputbase.pdf')
page = doc[0]
rect = fitz.Rect(181,3226,1813,3358)
page.drawRect(rect)
doc.save('latest.pdf')
page.drawRect(50,100,200,300)
rect = fitz.Rect(50, 100, 200,300)
page.drawRect(rect)
doc.save('latest.pdf')
pip freeze
import PIL
im = Image.open('sample.tiff')
from PIL implrt Image, ImageDraw
from PIL import Image, ImageDraw
im = Image.open('sample.tiff')
draw = ImageDraw.Draw(im)
draw.rectangle([181, 3226, 1813, 3260], outline=rgd(255,0,0))
draw.rectangle([181, 3226, 1813, 3260], outline=rgb(255,0,0))
from PIL import ImageColor
draw.rectangle([181, 3226, 1813, 3260], outline=ImageColor.rgb(255,0,0))
draw.rectangle([181, 3226, 1813, 3260], outline=ImageColor(255,0,0))
draw.rectangle([181, 3226, 1813, 3260], outline="black")
im.save('rectangle.png')
from bs4 import BeautifulSoup
soup = BeautifulSoup()
soup = BeautifulSoup('outputbase.hoce', 'html.parser')
soup.findAll('p')
soup = BeautifulSoup('outputbase.hocr', 'html.parser')
with open('outputbase.hocr','r') as f:
    hocr = f.read()
    
soup = BeautifulSoup(hocr, 'html.parser')
soup.findAll('p')
for p in soup.findAll('p'):
    print(p['class'])
    
for p in soup.findAll('p'):
    print(p['title'])
    
for p in soup.findAll('p'):
    print(p['title'].split(' '))
    
for p in soup.findAll('p'):
    print(p['title'].split(' ')[1:])
    
for p in soup.findAll('p'):
    draw.rectangle(p['title'].split(' ')[1:], outline="black")
    
for p in soup.findAll('p'):
    coordinates = p['title'].split(' ')[1:]
    print(coordinates)
    draw.rectangle(coordinates, outline="black")
    
for p in soup.findAll('p'):
    coordinates = list(p['title'].split(' ')[1:])
    print(coordinates)
    draw.rectangle(coordinates, outline="black")
    
for p in soup.findAll('p'):
    coordinates = set(p['title'].split(' ')[1:])
    print(coordinates)
    draw.rectangle(coordinates, outline="black")
    
for p in soup.findAll('p'):
    coordinates = (p['title'].split(' ')[1:])
    print(coordinates)
    draw.rectangle(coordinates, outline="black")
    
im = Image.open('sample.tiff')
draw = ImageDraw.Draw(im)
for p in soup.findAll('p'):
    coordinates = (p['title'].split(' ')[1:])
    print(coordinates)
    draw.rectangle(coordinates, outline="black")
    
for p in soup.findAll('p'):
    coordinates = tuple(p['title'].split(' ')[1:])
    print(coordinates)
    draw.rectangle(coordinates, outline="black")
    
for p in soup.findAll('p'):
    coordinates = p['title'].split(' ')[1:2], p['title'].split(' ')[3:4]
    print(coordinates)
    draw.rectangle(coordinates, outline="black")
    
    
draw.rectangle([181, 3226, 1813, 3260], outline="black")
for p in soup.findAll('p'):
    coordinates = (p['title'].split(' ')[1:])
    print(coordinates)
        
for p in soup.findAll('p'):
    coordinates = (p['title'].split(' ')[1:])
    points = []
    for point in coordinates:
        points.append(int(point))
        
points
for p in soup.findAll('p'):
    coordinates = (p['title'].split(' ')[1:])
    points = []
    for point in coordinates:
        points.append(int(point))
    draw.rectangle(points, outline='black')
    
im.save('newnew.png')
%save -r mysession 1-99999


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
