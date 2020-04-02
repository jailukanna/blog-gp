---
title: PIL example download (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'download'


## python download

Python PIL example: download

```python
#!/usr/bin/env python
"""
Generate links to download MODIS Flood Maps from Goddard

Usage:

python download.py >> links.txt
wget -i links.txt

Requisites:
        python
        wget

"""
# define the bounding variables.
site = "http://oas.gsfc.nasa.gov"

viewports = [
            '010E000S',
            '020E000S',
            '030E000S',
            '040E000S',
            '010E010S',
            '020E010S',
            '030E010S',
            '040E010S',
            '010E020S',
            '020E020S',
            '030E020S',
            '040E020S',
            '010E030S',
            '020E030S',
]


timespan = [ 2013001, # jan 1st
             2013028] # jan 28 (actually 27, since it only has data until yesterday)

filename_templates = ["/Products/%(viewport)s/MWP_%(time)s_%(viewport)s_3D3OT.tif",]

# Iterate over viewports and times to get the urls
for viewport in viewports:
    for delta in range(timespan[1]-timespan[0]):
        time = timespan[0] + delta
        for f in filename_templates:
            path = f % {'viewport': viewport, 'time': time}
            url = site + path
            print(url)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
