---
title: matplotlib example arreglo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'arreglo'


Modules used in program: 
* `import os`
* `import numpy as np`

## python arreglo

Python matplotlib example: arreglo

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-


import numpy as np
import os

os.system('cls')

a = np.arange(15).reshape(3,5)
print("Arreglo:\n",a)
print("Shape:",a.shape)
print("Dim:",a.ndim) 
print("Nombre:",a.dtype.name)
print("Tam:",a.itemsize)
print("Size:",a.size)
print("Type:",type(a))

arreglo = np.array([1,2,3,4,5,6])
print("Arreglo:",arreglo," , tipo:",arreglo.dtype)
arreglo = np.array([0.6,45.3,23.1,0.0999,3.21])
print("Arreglo:",arreglo," , tipo:",arreglo.dtype)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
