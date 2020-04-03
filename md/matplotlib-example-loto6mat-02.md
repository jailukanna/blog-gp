---
title: matplotlib example loto6mat-02 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'loto6mat-02'


Modules used in program: 
* `import csv`
* `import numpy as np`
* `import matplotlib.pylab as plt`
* `import random`

## python loto6mat-02

Python matplotlib example: loto6mat-02

```python
import random
import matplotlib.pylab as plt
import numpy as np
import csv

k = [0]*43
luckey_numbers = []
Lu_Nu = []
a = [0]*43

for i in range(43):
  a[i] = i+1

#csv_reader = csv.reader(open("loto6-10.txt","r",encoding='utf-8'))
f = fopen(loto6-30.txt)
b = f.read()
#fclose()
#header = next(csv_reader)
#print('\n'.join(header))

for row in csv_reader:
  c = row[3:-12]
  b = list(map(int,c))
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
  
  #Lu_Nu = luckey_numbers

  #print('sort = ',sorted(Lu_Nu))
print('k = ',k)
  #print()
x = a
y = k
value = y

plt.scatter(x,y,s=30, c=value, cmap='OrRd')
plt.colorbar()
plt.title("Loto6")
plt.xlabel("X")
plt.ylabel("Y")
plt.grid(which='major',color='blue',linestyle='-')
plt.xlim(1,43)
#plt.plot(c)
plt.show()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
