---
title: mysql example aleatoriedad (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'aleatoriedad'


Modules used in program: 
* `import random`

## python aleatoriedad

Python mysql example: aleatoriedad

```python
import random

four_rand_numb=[random.random() for _ in range(500)]
print(four_rand_numb)
print(random.shuffle(four_rand_numb))
print(len(four_rand_numb))

a=range(200)
print(a)
number=random.choice(a)
print(number)
#practicando un poco lo concerniente a los diccionarios
list1=[1,2,3,4,5,6,7,8]
list2=['alejo','carpentier','jose','marti','antonio','maceo','vicente','garcia']
f=dict(zip(list2,list1))
print(f)
numberList = [1, 2, 3]
strList = ['one', 'two', 'three']
d=dict(zip(numberList,strList))
print(d)
f["alejo"]=99
print(f)
alejos_f=f.get('alejo',0)
print(alejos_f)
s=random.choice(list2)
print(s)
print("llave de ",s," es: ",f.get(s))
print(f.keys(),"\n",f.values(),"\n", f.items())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
