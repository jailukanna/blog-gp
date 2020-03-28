---
title: tkinter example gui test treeview (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'gui test treeview'

Functions in program: 
* `def case_data(interval=1):`
* `def normalized(word):`
* `def randomDate(prop, start="1/1/2000 1:30 PM", end="1/1/2017 4:50 AM"):`
* `def strTimeProp(start, end, format, prop):`

Modules used in program: 
* `import random, time`

## python gui test treeview

Python tkinter example: gui test treeview

```python
import random, time


def strTimeProp(start, end, format, prop):

    stime = time.mktime(time.strptime(start, format))
    etime = time.mktime(time.strptime(end, format))
    ptime = stime + prop * (etime - stime)

    return time.strftime(format, time.localtime(ptime))


def randomDate(prop, start="1/1/2000 1:30 PM", end="1/1/2017 4:50 AM"):
    return strTimeProp(start, end, '%m/%d/%Y %I:%M %p', prop)


def normalized(word):
    return word.strip().replace('_', '').capitalize()


def case_data(interval=1):

    test_case = []
    counter = 0

    with open('C:/Users/KKhamutou/IdeaProjects/WikiProject/words', 'r') as words:
        for line in words:
            counter += 1
            if counter % interval == 0:
                test_case.append([counter, random.randint(10000000, 99999999),
                                  normalized(line), line, randomDate(random.random())]
                                 )
    return tuple(test_case)




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
