---
title: PIL example mandel (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'mandel'


## python mandel

Python PIL example: mandel

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

class MandelPoint:

    __slots__ = ('x', 'y')

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def steps_to_dist(self, dist, maxsteps):
        x = self.x
        y = self.y
        for i in range(maxsteps):
            if (x**2 + y**2)**0.5 > dist:
                break
            x = x**2 - y**2 + self.x
            y = 2*x*y + self.y
        return i


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
