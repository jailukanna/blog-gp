---
title: pil example NumPy2PIL (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'NumPy2PIL'


Modules used in program: 
* `import PIL`
* `import numpy`

## python NumPy2PIL

Python pil example: NumPy2PIL

```python
import numpy
import PIL
 
# Convert Image to array
img = PIL.Image.open("foo.jpg").convert("L")
arr = numpy.array(img)
 
# Convert array to Image
img = PIL.Image.fromarray(arr)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
