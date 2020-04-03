---
title: matplotlib example determinant (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'determinant'


Modules used in program: 
* `import numpy`

## python determinant

Python matplotlib example: determinant

```python
import numpy
from numpy.linalg import det

a = [[1,2], [-2,1], [2,5], [1,1]]
b = [[1,2], [0.5, 3]]
c = [[1,4,2,1],[3,2,0,1]]

# Násobení A*B
ab = numpy.dot(a,b)

# Násobení A*B*C
abc = numpy.dot(ab,c)

detabc = numpy.linalg.det(abc)

# Determinant ABC
print(round(detabc,3))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
