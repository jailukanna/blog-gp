---
title: matplotlib example loto6 LineGraph (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'loto6 LineGraph'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python loto6 LineGraph

Python matplotlib example: loto6 LineGraph

```python
#txt10~30をグラフ表示
import matplotlib.pyplot as plt
import numpy as np

k = [0]*43
kk = [0]*43
kkk = [0]*43
numbers1 = []
numbers2 = []
numbers3 = []
a = [0]*43

for i in range(43):
  a[i] = i+1
x = a

for line1 in open("loto6-10.txt"):
  if line1[0] == "#":
    continue
  c = line1.split()
  b = list(map(int,c))

  numbers1 = b

  for i in range(44):
    if numbers1[0] == i:
      k[i-1] += 1
    elif numbers1[1] == i:
      k[i-1] += 1
    elif numbers1[2] == i:
      k[i-1] += 1
    elif numbers1[3] == i:
      k[i-1] += 1
    elif numbers1[4] == i:
      k[i-1] += 1
    elif numbers1[5] == i:
      k[i-1] += 1
y1 = k

for line2 in open("loto6-20.txt"):
  if line2[0] == "#":
    continue
  cc = line2.split()
  bb = list(map(int,cc))

  numbers2 = bb

  for i in range(44):
    if numbers2[0] == i:
      kk[i-1] += 1
    elif numbers2[1] == i:
      kk[i-1] += 1
    elif numbers2[2] == i:
      kk[i-1] += 1
    elif numbers2[3] == i:
      kk[i-1] += 1
    elif numbers2[4] == i:
      kk[i-1] += 1
    elif numbers2[5] == i:
      kk[i-1] += 1
y2 = kk

for line3 in open("loto6-30.txt"):
  if line3[0] == "#":
    continue
  ccc = line3.split()
  bbb = list(map(int,ccc))

  numbers3 = bbb

  for i in range(44):
    if numbers3[0] == i:
      kkk[i-1] += 1
    elif numbers3[1] == i:
      kkk[i-1] += 1
    elif numbers3[2] == i:
      kkk[i-1] += 1
    elif numbers3[3] == i:
      kkk[i-1] += 1
    elif numbers3[4] == i:
      kkk[i-1] += 1
    elif numbers3[5] == i:
      kkk[i-1] += 1
y3 = kkk

plt.scatter(x,y1,s=30,color='red')
#plt.scatter(x,y2,s=30,color='lightgreen')
plt.scatter(x,y3,s=30,color='black')
plt.title("loto6")
plt.plot(x,y1,label='txt10',color='red')
plt.plot(x,y2,label='txt20',color='lightgreen')
plt.plot(x,y3,label='txt30',color='black')
plt.xlim(1,43)
plt.grid(which='major',color='yellow',linestyle='-')
plt.legend()
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
