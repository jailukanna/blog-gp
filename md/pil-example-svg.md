---
title: pil example svg (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'svg'

Functions in program: 
* `def find_between( s, first, last ):`

Modules used in program: 
* `import cairo`
* `import Image`
* `import rsvg`

## python svg

Python pil example: svg

```python
# http://stackoverflow.com/questions/10177985/svg-rounded-corner

def find_between( s, first, last ):
    try:
        start = s.index( first ) + len( first )
        end = s.index( last, start )
        return s[start:end]
    except ValueError:
        return ""

import rsvg
import Image
import cairo

d='<g transform="translate(304 383.5)"><path width="360" height="670" d="M 14 4 L 14 40 L 14 50" style="stroke: rgb(50,50,255); stroke-width: 10; fill: none; opacity: 1; " stroke-linejoin="round"  transform="translate(-18 -33.5)" /></g>'
nopos=find_between(d,">","transform=")+'transform="translate(50 50)" />'
data='<svg>'+nopos+'</svg>'
print(data)
svg = rsvg.Handle(data=data)
x = width = svg.props.width
y = height = svg.props.height

newWidth=width+100
newHeight=height+100

# scale http://stackoverflow.com/questions/1187358/how-to-resize-svg-image-file-using-librsvg-python-binding
surface = cairo.ImageSurface(cairo.FORMAT_ARGB32, newWidth, newHeight)
context = cairo.Context(surface)
svg.render_cairo(context)
pilImage = Image.frombuffer('RGBA',(newWidth,newHeight),surface.get_data(),'raw','RGBA',0,1)
pilGray=pilImage.convert('L')
pilGray.save('test.png')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
