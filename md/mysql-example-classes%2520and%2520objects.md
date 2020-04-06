---
title: mysql example classes%2520and%2520objects (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'classes%2520and%2520objects'


## python classes%2520and%2520objects

Python mysql example: classes%2520and%2520objects

```python
class Enemy:
    life = 3

    def attack(self):
        print('ouch!')
        self.life-=1

    def checklife(self):
        if self.life <=0 :
            print('I am dead')
        else :
            print(str(self.life) + "life left")

enemy1= Enemy()
enemy2= Enemy()

enemy1.attack()
enemy1.attack()
enemy1.checklife()
enemy2.checklife()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
