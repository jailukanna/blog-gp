---
title: turtle example ListFunctionsAndDictionaries (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'ListFunctionsAndDictionaries'


Modules used in program: 
* `import random`

## python ListFunctionsAndDictionaries

Python turtle example: ListFunctionsAndDictionaries

```python
import random
'''
list3=[1,3.14,'abc',[4,5,6],'pqr']
x=0.99
list3.append(x)
print((list3))

list3.reverse()
print(list3)

list3.remove('abc')
print(list3)

list3.pop(3)
print(list3)

list3.insert(len(list3),'abc')
print(list3)
'''
'''
list1=[1 for i in range(13)]
for x in range(100000000):
    die1=random.randint(1,6)
    die2=random.randint(1,6)
    Sum=die1+die2
    list1[Sum]=list1[Sum]+1

for x in range(2,13):
    print("prob :",x,"is",list1[x]/100000000)
 '''
'''
str1="xx-xx-xxxxxx"
words=str1.split("-")
print(words)

list3=['A','B','C']
print(' '.join(list3))
print(''.join(list3))
print(','.join(list3))
print('--'.join(list3))
print('**'.join(list3))
'''
'''
list2=[]
list4=[]
list1=[1,14,5,9,12]
for x in list1:
    if x<10:
        list2.append(x)
print(list2)

list3=['one','two','three','four','five','six']
for x in list3:
    if len(x)== 3:
        list4.append(x[0])
print(list4)
'''
'''
list5=[[i,j] for i in range(2) for j in range(3)]
d1={'A':10,'B':20}
list1=[10,20]
d1['A']=30
list2[2]=40
'''
'''
deck=[{'values':i,'suit':c} for c in ['spades','clubs','hearts','diamonds']for i in range(2,15)]
print(deck[0]['values'],' of ',deck[0]['suit'])

help(random.shuffle)
random.shuffle(deck)
print(deck[0]['values'],' of ',deck[0]['suit'])

numerals={'I':1,'V':5}

for key in numerals:
        print(key,"equals",numerals[key])

numerals.get('I')
help(dict.get)
 '''   

d1={'name':"Jivitesh",5:"lol"}
for keys,pairs in d1.items():
    print(keys,":",pairs)

for count in range(6, 1, -1):
    print(count, end = " ")









































 
    
    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
