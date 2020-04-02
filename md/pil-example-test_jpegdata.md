---
title: pil example test jpegdata (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'test jpegdata'


Modules used in program: 
* `import base64`

## python test jpegdata

Python pil example: test jpegdata

```python
from PIL import Image
from io import BytesIO
import base64

data = 'R0lGODlhDwAPAKECAAAAzMzM/////wAAACwAAAAADwAPAAACIISPeQHsrZ5ModrLlN48CXF8m2iQ3YmmKqVlRtW4MLwWACH+H09wdGltaXplZCBieSBVbGVhZCBTbWFydFNhdmVyIQAAOw=='
bytesdata = BytesIO(base64.b64decode(data))

im = Image.open(bytesdata)
im.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
