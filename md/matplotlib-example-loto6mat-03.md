---
title: matplotlib example loto6mat-03 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'loto6mat-03'


Modules used in program: 
* `import csv`
* `import numpy as np`
* `import matplotlib.pylab as plt`

## python loto6mat-03

Python matplotlib example: loto6mat-03

```python
import matplotlib.pylab as plt
import numpy as np
import csv

k = [0]*43
luckey_numbers = []
Lu_Nu = []
a = [0]*43
#b = []
#lines = []
#c = []
n = []

for i in range(43):
  a[i] = i+1

for line in open("loto6-10.txt"):
  #if line[0] == "#":
    #continue
  c = line.split()#文字列を空白区切分割
  b = list(map(int,c))#文字列を整数に変換

  print('b=',b)
  luckey_numbers = b

  for i in range(44):
    if luckey_numbers[0] == i:
      k[i-1] += 1
    elif luckey_numbers[1] == i:
      k[i-1] += 1
    elif luckey_numbers[2] == i:
      k[i-1] += 1
    elif luckey_numbers[3] == i:
      k[i-1] += 1
    elif luckey_numbers[4] == i:
      k[i-1] += 1
    elif luckey_numbers[5] == i:
      k[i-1] += 1
  
print('k = ',k)
  
x = a
y = k
value = y

plt.plot(x,y)
plt.title("Loto6")
plt.xlabel("X")
plt.ylabel("Y")
plt.grid(which='major',color='green',linestyle='-')
plt.ylim(0)
plt.xlim(1,43)
#plt.plot(x,y)
plt.show()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
