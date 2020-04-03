---
title: matplotlib example loto6 timebar (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'loto6 timebar'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pylab as plt`

## python loto6 timebar

Python matplotlib example: loto6 timebar

```python
#txt10~30 バー表示
import matplotlib.pylab as plt
import numpy as np

k = [0]*43
kk = [0]*43
kkk = [0]*43
luckey_numbers1 = []
luckey_numbers2 = []
luckey_numbers3 = []
a = [0]*43

for i in range(43):
  a[i] = i+1
  
x = a

for line1 in open("loto6-10.txt"):
  if line1[0] == "#":
    continue
  c = line1.split()
  b = list(map(int,c))#文字列を整数に変換

  luckey_numbers1 = b

  for i in range(44):
    if luckey_numbers1[0] == i:
      k[i-1] += 1
    elif luckey_numbers1[1] == i:
      k[i-1] += 1
    elif luckey_numbers1[2] == i:
      k[i-1] += 1
    elif luckey_numbers1[3] == i:
      k[i-1] += 1
    elif luckey_numbers1[4] == i:
      k[i-1] += 1
    elif luckey_numbers1[5] == i:
      k[i-1] += 1
  
#x = a
y1 = k

for line2 in open("loto6-20.txt"):
  if line2[0] == "#":
    continue
  cc = line2.split()
  bb = list(map(int,cc))#文字列を整数に変換

  luckey_numbers2 = bb

  for i in range(44):
    if luckey_numbers2[0] == i:
      kk[i-1] += 1
    elif luckey_numbers2[1] == i:
      kk[i-1] += 1
    elif luckey_numbers2[2] == i:
      kk[i-1] += 1
    elif luckey_numbers2[3] == i:
      kk[i-1] += 1
    elif luckey_numbers2[4] == i:
      kk[i-1] += 1
    elif luckey_numbers2[5] == i:
      kk[i-1] += 1  
y2 = kk

for line3 in open("loto6-30.txt"):
  if line3[0] == "#":
    continue
  ccc = line3.split()
  bbb = list(map(int,ccc))#文字列を整数に変換

  luckey_numbers3 = bbb

  for i in range(44):
    if luckey_numbers3[0] == i:
      kkk[i-1] += 1
    elif luckey_numbers3[1] == i:
      kkk[i-1] += 1
    elif luckey_numbers3[2] == i:
      kkk[i-1] += 1
    elif luckey_numbers3[3] == i:
      kkk[i-1] += 1
    elif luckey_numbers3[4] == i:
      kkk[i-1] += 1
    elif luckey_numbers3[5] == i:
      kkk[i-1] += 1
y3 = kkk
bar_width = 0.2
bar_width1 = 0.4
bar_width2 = 0.2

X = np.array(a)
Y1 = np.array(y1)
Y2 = np.array(y2)
Y3 = np.array(y3)

plt.title("Loto6")
plt.bar(X,Y1,width=bar_width,color="gray",label="txt10",align='center')
plt.bar(X+bar_width1,Y2,width=bar_width1,color="lightblue",label="txt20",align='center')
plt.bar(X+bar_width2,Y3,width=bar_width2,color="pink",label="txt30",align='center')

plt.grid(which='major',color='yellow',linestyle='-')
plt.legend()

#plt.xlim(1,43)

plt.show()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
