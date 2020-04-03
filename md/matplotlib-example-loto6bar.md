---
title: matplotlib example loto6bar (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'loto6bar'


Modules used in program: 
* `import csv`
* `import numpy as np`
* `import matplotlib.pylab as plt`

## python loto6bar

Python matplotlib example: loto6bar

```python
import matplotlib.pylab as plt
import numpy as np
import csv

k = [0]*43
luckey_numbers = []
Lu_Nu = []
a = [0]*43

for i in range(43):
  a[i] = i+1
csv_reader = csv.reader(open("loto6.csv","r",encoding='cp932'))

header = next(csv_reader)
print('\n'.join(header))

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
  
  Lu_Nu = luckey_numbers

print('k = ',k)
  
x = a
y = k
value = y
left = np.array(x)
height = np.array(y)

#plt.scatter(x,y,s=30, c=value, cmap='OrRd')
#plt.colorbar()
plt.title("Loto6")
plt.xlabel("X")
plt.ylabel("Y")
plt.bar(left,height,color="lightgreen",align="center")
plt.grid(which='major',color='yellow',linestyle='-')
plt.xlim(1,43)
#plt.plot(x,y)
plt.show()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
