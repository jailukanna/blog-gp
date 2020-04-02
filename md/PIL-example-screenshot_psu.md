---
title: PIL example screenshot psu (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'screenshot psu'


Modules used in program: 
* `import requests`
* `import sys`
* `import io`

## python screenshot psu

Python PIL example: screenshot psu

```python
import io
import sys
import requests
from PIL import Image

if len(sys.argv) != 2:
    print("Usage: screenshot_psu.py <filename>")
    sys.exit(1)

# first log in
requests.get("http://psu.bench/cgi/login?pass=keysight")
# now download screenshot
r = requests.get("http://psu.bench/get/screenshot.bmp")
# convert to PNG and save
img = Image.open(io.BytesIO(r.content))
img.save(sys.argv[1])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
