---
title: matplotlib example numpy (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'numpy'

Functions in program: 
* `def main():`
* `def regresa_arreglo(matrix):`

Modules used in program: 
* `import os`
* `import numpy as np`

## python numpy

Python matplotlib example: numpy

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

#Numpy
import numpy as np
import os

def regresa_arreglo(matrix):
    arreglo = np.array(matrix,dtype=float)
    return arreglo

def main():
    os.system('cls')
    arreglo = np.array([1,2,3,4,5,6])
    print(arreglo)
    my_matrix = [[3,2,1],[8,7,6]]
    print("Matrix:",my_matrix)
    print("Regresa arreglo:",regresa_arreglo(my_matrix))

if __name__ == '__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
