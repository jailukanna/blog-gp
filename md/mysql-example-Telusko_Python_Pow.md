---
title: mysql example Telusko Python Pow (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Telusko Python Pow'

Functions in program: 
* `def deco(func):`
* `def som():`

Modules used in program: 
* `import numpy`
* `import math as m`
* `import sys as s`

## python Telusko Python Pow

Python mysql example: Telusko Python Pow

```python
import sys as s
import math as m
from functools import reduce
import numpy

"""x = int(s.argv[1])
print(m.pow(x, 3))
ar = ['i',('2','3')]

print(ar)"""

"""def data(name,**data):
    print(name)
    for i,j in data.items():
        print(i,j)




data('mukund',age = '25',city = 'hyderabad', mob = 9030396887)"""


"""a = 9

print(id(a))


def som():
    a=5
    print(id(a))

    x = globals()['a']
    print(id(x))
    print(x)

    print(('local: ',a))
    globals()['a'] = 78


som()

print('global: ',a)"""


lst = ['mukund','robert','jay','syeraa','tom','jerry']

"""def count(lst):
    even = 0
    odd = 0


    for l in lst:
        if len(l)>=5:
           even+=1
        else:
            odd+=1

    return even,odd


even, odd = count(lst)

print("Even : {} and Odd : {}".format(even,odd))


print(s.getrecursionlimit())"""


"""f= lambda a,b: a+b

res = f(2,3)

print(res)"""


"""lst = [2,4,6,7,8,23,56]

evens = (list(filter(lambda n: n%2==0,lst)))

map = (list(map(lambda n : n*3,evens)))

reduce = reduce(lambda a,b: a+b*2,map)

print(reduce)"""


"""def div(a,b):
    print(a/b)


def deco(func):
    def inner(a,b):

        if a<b:
            a,b = b,a

        return func(a,b)
    return inner


div = deco(div)
div(4,9)"""


print("Hello: "+__name__)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
