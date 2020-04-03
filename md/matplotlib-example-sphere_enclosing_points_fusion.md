---
title: matplotlib example sphere enclosing points fusion (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sphere enclosing points fusion'


Modules used in program: 
* `import plot`
* `import numpy as np`
* `import mosek `
* `import sys`

## python sphere enclosing points fusion

Python matplotlib example: sphere enclosing points fusion

```python
import sys

import mosek 
from  mosek.fusion import *
import numpy as np

import plot

k=int(sys.argv[1]) #number of points
n=int(sys.argv[2]) #dimension

if n<2:
    print("the dimension must be greater than one!")
    exit(1)

print('Generate '+str(k)+' points in R^'+str(n)+' normally distributed around the origin...')
p=  np.random.normal(size=(k,n))
print('done!\n')

print('Creating the Fusion optimization model...')
M = Model("minimal sphere enclosing a set of points")
print('done!\n')

print('Declaring the variables...')
r = M.variable("r", Domain.unbounded())
p0 = M.variable("p_0", NDSet(1,n), Domain.unbounded())
print('done!\n')

print('Defining the constraints...')
M.constraint('norm', Expr.hstack( [ Expr.mul(np.ones(k),r), Expr.sub(Expr.mul(np.ones( (k,1) ), p0 ), DenseMatrix(p) ) ] ), Domain.inQCone(k,n+1))
print('done!\n')

print('Defining the objective function...')
M.objective('min_radius', ObjectiveSense.Minimize, r)
print('done!\n')

M.setLogHandler(sys.stdout) 
M.solve()
print("\nSolution status= "+ str(M.getPrimalSolutionStatus()))

rstar= r.level()[0]
pstar= p0.level()

print("r*= %f" % rstar)
print("p*= ")
print(pstar)

#comment out this block if you haven't matplotlib installed
if n==2:
    plot.plot_points(p,pstar,rstar)
#------------------------------------------------------------


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
