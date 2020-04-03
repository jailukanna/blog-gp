---
title: matplotlib example sovle matplot Chinese%2520 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sovle matplot Chinese%2520'


Modules used in program: 
* `import matplotlib.pyplot as plt `
* `import matplotlib`

## python sovle matplot Chinese%2520

Python matplotlib example: sovle matplot Chinese%2520

```python

# -*- coding:utf-8 -*-
import matplotlib
import matplotlib.pyplot as plt 
from matplotlib.font_manager import *  


matplotlib.use('qt4agg')    
matplotlib.rcParams['axes.unicode_minus']=False  
font = FontProperties(fname='path/font.ttc') 
plt.bar([1], [3])
plt.xticks([1], ['测试'], ,fontproperties=font)
plt.title(u'哈哈',fontproperties=font)  
plt.show() 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
