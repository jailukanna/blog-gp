---
title: mysql example data lake test test (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data lake test test'


Modules used in program: 
* `import math`
* `import heapq `

## python data lake test test

Python mysql example: data lake test test

```python
'''
heaqp模块提供了堆队列算法的实现，也称为优先级队列算法。
要创建堆，请使用初始化为[]的列表，或者可以通过函数heapify（）将填充列表转换为堆。
提供以下功能：
heapq.heappush（堆，项目）
将值项推入堆中，保持堆不变。
heapq.heapify（x）
在线性时间内将列表x转换为堆。
heapq.heappop（堆）
弹出并返回堆中的最小项，保持堆不变。如果堆是空的，则引发IndexError。
'''
import heapq 
import math
#1 heappush生成堆+ heappop把堆从小到大pop出来 
heap = []
data = [1,3,5,7,9,2,4,6,8,0,10,1]
for i in data:
    heapq.heappush(heap,i)
print(heap)

lis = []
while heap:
    lis.append(heapq.heappop(heap))
print('lis',lis)

#2 heapify生成堆+ heappop把堆从小到大pop出来 
data2 = [1,5,3,2,9,5]
heapq._heapify_max(data2)

print('data2[-1]',heapq._heappop_max(data2))
lis.clear()
while data2:
   lis.append(abs(heapq.heappop (data2)))
print(lis)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
