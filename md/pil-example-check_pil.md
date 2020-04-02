---
title: pil example check pil (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'check pil'

Functions in program: 
* `def check_pil(filename):`

## python check pil

Python pil example: check pil

```python
def check_pil(filename):
    from PIL import Image
    f = open(filename, 'r')
    img = Image.open(f)
    img.load()
    if hasattr(f, 'reset'):
        f.reset()
    img = Image.open(f)
    img.verify()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
