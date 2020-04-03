---
title: matplotlib example loto6mat2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'loto6mat2'


Modules used in program: 
* `import csv`
* `import numpy as np`
* `import matplotlib.pylab as plt`

## python loto6mat2

Python matplotlib example: loto6mat2

```python
import matplotlib.pylab as plt
import numpy as np
import csv

k = [0]*43
kk = [0]*43
luckey_numbers1 = []
luckey_numbers2 = []
Lu_Nu = []
a = [0]*43

'''
for i in range(43):
  a[i] = i+1
'''
csv_reader = csv.reader(open("loto6.csv","r",encoding='cp932'))

for line in open("loto6-10.txt"):
  #if line[0] == "#":
    #continue
  cc = line.split()#文字列を空白区切分割
  bb = list(map(int,cc))#文字列を整数に変換
  print('bb=',bb)
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
     
x = a
y1 = kk
value = y1
'''
plt.plot(x,y)
plt.title("Loto6")
plt.xlabel("X")
plt.ylabel("Y")
plt.grid(which='major',color='green',linestyle='-')
plt.ylim(0)
plt.xlim(1,43)
#plt.plot(x,y)
plt.show()
'''
header = next(csv_reader)
print('\n'.join(header))

for row in csv_reader:
  c = row[3:-12]
  b = list(map(int,c))
  print('b=',b)
  
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
  
  #Lu_Nu = luckey_numbers

  #print('sort = ',sorted(Lu_Nu))
print('k = ',k)
  #print()
x = a
y2 = k
value = y2

#plt.scatter(x,y,c=value, cmap='OrRd')
#plt.colorbar()
plt.plot(x,y1,label='y1')
plt.plot(x,y2)
#plt.title("Loto6")
#plt.xlabel("X")
#plt.ylabel("Y")
plt.grid(which='major',color='blue',linestyle='-')
plt.xlim(1,43)
#plt.plot(x,y1,color='green')
#plt.plot(x,y2,color='red')
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
