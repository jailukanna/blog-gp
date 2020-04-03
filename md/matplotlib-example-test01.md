---
title: matplotlib example test01 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'test01'


Modules used in program: 
* `import glob`
* `import numpy as np`
* `import matplotlib.pyplot as plt`

## python test01

Python matplotlib example: test01

```python
import matplotlib.pyplot as plt
import numpy as np
import glob

k = [0]*43
kk = [0]*43
kkk = [0]*43
numbers1 = []
numbers2 = []
numbers3 = []
line3 = []
a = [0]*43
b = 0

line1 = []

for i in range(43):
  a[i] = i+1
x = a

f = open('loto6.txt')
for line in f:
	line1.append(line)
	
for i in range(0,30):
	print(line1[i])
#f.close()
	c = line1[i].split()
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

plt.scatter(x,y1,s=30,color='red')
#plt.scatter(x,y2,s=30,color='green')
#plt.scatter(x,y3,s=30,color='black')
plt.title("loto6")
plt.plot(x,y1,label='past30',color='red')
#plt.plot(x,y2,label='past10',color='lightgreen')
#plt.plot(x,y3,label='pastt30',color='black')
plt.xlim(1,43)
plt.grid(which='major',color='yellow',linestyle='-')
plt.legend()
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
