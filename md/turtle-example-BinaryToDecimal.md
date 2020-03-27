---
title: turtle example BinaryToDecimal (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'BinaryToDecimal'


## python BinaryToDecimal

Python turtle example: BinaryToDecimal

```python
binary=int(input("enter your binary string: "))
power=0
decimal=0
while binary>0:
    decimal+=2**power*(binary%10)
    binary//=10
    power+=1
print("decimal: " ,decimal)
    
  


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
