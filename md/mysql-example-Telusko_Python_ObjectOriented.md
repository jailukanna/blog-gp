---
title: mysql example Telusko Python ObjectOriented (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Telusko Python ObjectOriented'

Functions in program: 
* `def sort(lst):`
* `def search(n):`

## python Telusko Python ObjectOriented

Python mysql example: Telusko Python ObjectOriented

```python


"""class Computer:
    school = "Cssi"

    def __init__(self,cpu):

        self.cpi = cpu
        return print("init")

    def config(self):
        print("cpu: ",self.cpi)

    @classmethod
    def info(cls):
        return cls.school

    @staticmethod
    def staticm():
        print("This is a static method")


class Comp2(Computer):

    def pr2(self):
        super().__init__("i3")
        print("Child clss")

c1 = Comp2("i5")
c1.config()
c1.pr2()
c1.staticm()"""



"""class Maruti:
    def exec(self):
        print("Made in India")

class Toyota:
    def exec(self):
        print(("MADE IN Japan"))


class FourWheeler:
    def ride(self,duck):
        duck.exec()


fw = FourWheeler()

fw.ride(Toyota())
fw.ride(Maruti())"""


"""class Student:

    def __init__(self,m1,m2):
       self.m1 = m1
       self.m2 = m2

    def __sub__(self, other):
        sub1 = self.m1 - other.m1
        sub2 = self.m2 - other.m2
        s3 = Student(sub1,sub2)
        return s3

    def __str__(self):
        return '{} {} '.format(self.m1,self.m2)


s1 = Student(23,34)
s2 = Student(34,34)

s = s1-s2



print(s.__str__())"""



"""class TopTen:

    def __init__(self):
         self.num = 1


    def __iter__(self):
        return self


    def __next__(self):
        if self.num<=10:
            val = self.num
            self.num+=1
            return val
        else:
            raise StopIteration


values = TopTen()


print(next(values))

for i in values:
    print(i)"""



"""a = 4
b="sda"

try:
    print(a/b)
except ZeroDivisionError as e:
    f1 = open("add",'w')
    f1.write("Error: ",e)
    print("Error: ",e)

except TypeError as e:
    print("Error: ",e)"""




"""ind = 0
n=6

def search(n):

    for i in lst:

        if lst[i]==n:
            globals()["ind"] = i
            return True


    return False

lst = [1,2,6,4,5,6,8,9]

if search(n):
    print("Found at :",ind+1)
else:
    print("Not Found")"""




def sort(lst):
    for i in range(6):
        minpos = i
        for j in range(i,7):
            if lst[j]<lst[minpos]:
                minpos = j

        temp = lst[i]
        lst[i] = lst[minpos]
        lst[minpos] = temp

        print(lst)

lst = [5,7,3,6,2,8,1]

sort(lst)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
