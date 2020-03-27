---
title: turtle example wombo-combo First (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'wombo-combo First'


Modules used in program: 
* `import random`

## python wombo-combo First

Python turtle example: wombo-combo First

```python
import random
from turtle import forward, right, left, shape, exitonclick
# a = int(input('Zadej stranu ctverce a: '))
# if a < 0:
#     print("Jsi asi uplny vul!")
# elif a == 0:
#     print("Taky ne")
# else:
#     o = a*4
#     s = a**2
#     print(type(a))
#     print('Obvod ctverce je',o,'cm')
#     print('Obsah ctverce je',s,'cm2')
#     print('Obvod ctverce se stranou {} je: {} cm2.'.format(o,s))
#     print("A">"B")





vyber = ['kámen', 'nůžky', 'papír']
tah_pocitace = random.choice(vyber)
tah_cloveka = input('kámen, nůžky, nebo papír? ')


while tah_cloveka not in vyber:
    tah_cloveka = input('Znova: kámen, nůžky, nebo papír? ')
print("Pocitac zvolil {}".format(tah_pocitace))
if tah_cloveka == tah_pocitace:
        print('Plichta.')
elif tah_cloveka == 'kámen' and tah_pocitace == 'nůžky' or tah_cloveka == 'nůžky' and tah_pocitace == 'papír' or tah_cloveka == 'papír' and tah_pocitace == 'kámen':
        print('Vyhrál jsi!')
else:
    print('Počítač vyhrál.')

# Zelvi program
forward(50)
right(90)
forward(50)
left(90)
forward(50)
shape('turtle')
exitonclick()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
