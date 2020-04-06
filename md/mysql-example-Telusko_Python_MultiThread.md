---
title: mysql example Telusko Python MultiThread (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Telusko Python MultiThread'


## python Telusko Python MultiThread

Python mysql example: Telusko Python MultiThread

```python
from threading import *
from time import sleep

class Hello(Thread):

    def run(self):
        for i in range(5):
            print("Hi")
            sleep(0.5)

class Hi(Thread):
    def run(self):
        for i in range(4):
            print("Hello")
            sleep(1)

h = Hello()
h2 = Hi()

h.start()
h2.start()

h.join()
h2.join()

print("end")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
