---
title: matplotlib example scatter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'scatter'


## python scatter

Python matplotlib example: scatter

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from matplotlib import pyplot as plt

data = """1	0.841470984807896
0.54030230586814	0.287797828416081
-0.416146836547142	0.172313864573127
-0.989992496600445	0.830544794233449
-0.653643620863612	0.414369558091726"""

for line in data.split('\n'):
    x, y = map(lambda a: float(a), line.split('\t'))
    plt.scatter(x, y)

plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
