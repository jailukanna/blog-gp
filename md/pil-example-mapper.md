---
title: pil example mapper (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'mapper'

Functions in program: 
* `def map_mandel(out, nx, ny, ns, dist):`

## python mapper

Python pil example: mapper

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from __future__ import division

from itertools import product
from mandel import MandelPoint

def map_mandel(out, nx, ny, ns, dist):
    """
    calc on grid with size 2*dist by 2*dist
    """
    for i, j in product(range(nx), range(ny)):
        x = (i - nx // 2) / (nx // 2) * dist
        y = (j - ny // 2) / (ny // 2) * dist
        out[j, i] = MandelPoint(x, y).steps_to_dist(dist, ns) / ns


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
