---
title: mysql example Algebra linear (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Algebra linear'

Functions in program: 
* `def sum_of_squares(v):`
* `def dot(v, w):`
* `def is_diagonal(i,j):`
* `def make_matrix(k,m, entry_fn): #haciendo una matriz`
* `def get_column(A,j): #extrayendo la columna j de la matriz A`
* `def get_row(A,i): #extrayendo la fila i de la matriz A`
* `def shape(A):`
* `def scalar_multiply(c, v):`
* `def vector_sum(vectors):`
* `def vector_subtract(v,w):`
* `def vector_add(v,w):`

Modules used in program: 
* `import random`
* `import numpy as np`

## python Algebra linear

Python mysql example: Algebra linear

```python
from matplotlib import pyplot as plt
import numpy as np
import random
v=[1,23,73,54,15]
w=[13,71,48,99,110]
s=list(zip(v,w))
def vector_add(v,w):
    #sumando los elementos correspondientes de cada vector
    return [v_i+w_i 
            for v_i,w_i 
            in zip(v,w)]
print(vector_add(v,w))

def vector_subtract(v,w):
    return [-v_i+w_i 
            for v_i,w_i 
            in zip(v,w)]
print(vector_subtract(v,w))

plt.scatter(vector_add(v,w),vector_subtract(v,w))
plt.show()


#vectors=[vector_add(v,w),vector_subtract(v,w)]
def vector_sum(vectors):
    result=vectors[0]
    for vector in vectors[1:]:
        result=vector_add(result,vector)
        return result

#PARA MULTIPLICAR UN VECTOR POR UN ESCALAR:
c=int(input("entre un numero: "))
vec=list(input(" entre um vector:   "))
def scalar_multiply(c, v):
    return [c * v for v in vec]
print(scalar_multiply(c,vec))

#MATRICES
A=[[1,2,3],[4,5,6]]
B=[[1,2],[3,4],[5,6]]


#esto es para extraer el numero de columnas y dde filas
def shape(A):
    num_rows=len(A)
    num_cols=len(A[0]) if A else 0
    return num_rows, num_cols
def get_row(A,i): #extrayendo la fila i de la matriz A
    return A[i]
def get_column(A,j): #extrayendo la columna j de la matriz A
    return [A_i[j] for A_i in A]
l=shape(A)
k=get_row(A,0)
m=get_column(B,0)
n=np.array(B)
print(l,k,m)
print(n)
print("A=",np.array(A), "\n","B=",np.array(B))

def make_matrix(k,m, entry_fn): #haciendo una matriz
    return [[entry_fn(i,j)
             for j in range(m)]
            for i in range(k)]

def is_diagonal(i,j):
    return 1 if i==j else 0
print(is_diagonal(5,5))


def dot(v, w):
    """v_1 * w_1 + ... + v_n * w_n"""
    return sum(v_i * w_i for v_i, w_i in zip(v, w))

def sum_of_squares(v):
    """v_1 * v_1 + ... + v_n * v_n"""
    return dot(v, v)










```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
