---
title: PIL example render (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'render'


Modules used in program: 
* `import os`
* `import glob`
* `import mapnik`

## python render

Python PIL example: render

```python
#!/usr/bin/env python
import mapnik
import glob
import os


for f in glob.glob('*.tif'):
    map = mapnik.Map(9012, 9012)

    filename = os.path.abspath(f)

    basename, ext = os.path.splitext(filename)

    pngname = basename + '.png'
    stylename = basename + '.xml'

    with open(stylename, 'wt') as out:
        for line in open('merge.xml'):
            out.write(line.replace('REPLACE', filename))

    mapnik.load_map(map, stylename)
    map.zoom_all()
    mapnik.render_to_file(map, pngname)

    print(pngname)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
