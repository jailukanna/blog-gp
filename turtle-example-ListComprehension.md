---
title: turtle example ListComprehension (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'ListComprehension'


## python ListComprehension

Python turtle example: ListComprehension

```python
'''
x=input("enter your stirng: ")
str2=''
str3=''
for char in x:
    str2=str2+str(ord(char))
print(str2)
    
for chari in range(0,len(str2),2):
   number=int(str2[chari]+str2[chari+1])
   str3=str3+chr(number)
print(str3)
'''
'''
str4=''
l=input("enter")
for x in range(len(l)-1,-1,-1):
    str4=str4+l[x]
print(str4)
'''
str1='hello'
list1=[1,14,5,9,12]
list2=['one','two','three','four','five','six']

listTest=[m[0] for m in list2]
listTest2=[i for i in list1 if i<10]
listTest3=[m[0] for m in list2 if len(m)==3]
print(listTest2)




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
