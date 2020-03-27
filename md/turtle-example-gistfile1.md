---
title: turtle example gistfile1 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'gistfile1'

Functions in program: 
* `def main():`

Modules used in program: 
* `import random`
* `import math`

## python gistfile1

Python turtle example: gistfile1

```python
#修改turtle模組中的BYTEDESIGN，其實也只有改變化顏色的部分

import math
from turtle import Turtle, mainloop
from time import clock
import random

顏色對照表=dict()
顏色對照表[0]="Black"
顏色對照表[1]="Red"
顏色對照表[2]="Blue"
顏色對照表[3]="Green"
顏色對照表[4]="Yellow"


class 設計圖(Turtle):

    def 變化顏色(self):
        顏色代號 = random.randrange(0,5)
        self.color(顏色對照表[顏色代號])

    
    def 畫圖(self, 起點, 倍率, 模式=1):
        self.up()
        self.goto(起點)
        for i in range(5):
            
            self.變化顏色()
            self.forward(64.65 * 倍率)
            self.down()
            self.畫輪部件(self.position(), 倍率, 模式)
            self.up()
            self.backward(64.65 * 倍率)
            self.right(72)
            
        self.up()  
        self.right(36)
        self.forward(24.5 * 倍率)
        self.right(198)
        self.down()

        顏色代號 = random.randrange(0,5)
        self.color(顏色對照表[顏色代號])
        
        self.中央區塊(46 * 倍率, 143.4, 倍率)
        self.getscreen().tracer(True)
        

    def 畫輪部件(self, initpos, 倍率, 模式):
        self.right(54)

        if(模式==2):
            for i in range(4):
                self.變化顏色();
                self.畫多角區塊(initpos, 倍率,模式)
        else:
            for i in range(4):
                self.畫多角區塊(initpos, 倍率,模式)
            
        self.down()
        self.left(36)
        
        if(模式==2):
            for i in range(5):
                self.變化顏色();
                self.畫三角區塊(initpos, 倍率,模式)
        else:
            for i in range(5):
                self.畫三角區塊(initpos, 倍率,模式)

        self.left(36)

        for i in range(5):
            self.down()
            self.right(72)
            self.forward(5 * 倍率)
            self.up()
            self.backward(28 * 倍率)

        self.left(54)
        self.getscreen().update()

    def 畫三角區塊(self, initpos, 倍率,模式):
        oldh = self.heading()
        self.down()
        self.backward(2.5 * 倍率)
        self.畫右三角(31.5 * 倍率, 倍率,模式)
        self.up()
        self.goto(initpos)
        self.setheading(oldh)
        self.down()
        self.backward(2.5 * 倍率)
        self.畫左三角(31.5 * 倍率, 倍率,模式)
        self.up()
        self.goto(initpos)
        self.setheading(oldh)
        self.left(72)
        self.getscreen().update()

    def 畫多角區塊(self, initpos, 倍率,模式):
        oldh = self.heading()
        self.up()
        self.forward(29 * 倍率)
        self.down()
        for i in range(5):
            self.forward(18 * 倍率)
            self.right(72)
        self.畫右多角(18 * 倍率, 75, 倍率,模式)
        self.up()
        self.goto(initpos)
        self.setheading(oldh)
        self.forward(29 * 倍率)
        self.down()
        for i in range(5):
            self.forward(18 * 倍率)
            self.right(72)
        self.畫左多角(18 * 倍率, 75, 倍率,模式)
        self.up()
        self.goto(initpos)
        self.setheading(oldh)
        self.left(72)
        self.getscreen().update()

    def 畫左多角(self, side, ang, 倍率,模式):
        if(模式==3): self.變化顏色()
        
        if side < (2 * 倍率): return
        self.forward(side)
        self.left(ang)
        self.畫左多角(side - (.38 * 倍率), ang, 倍率,模式)

    def 畫右多角(self, side, ang, 倍率,模式):
        if(模式==3): self.變化顏色()
        
        if side < (2 * 倍率): return
        self.forward(side)
        self.right(ang)
        self.畫右多角(side - (.38 * 倍率), ang, 倍率,模式)

    def 畫右三角(self, side, 倍率,模式):
        if(模式==3): self.變化顏色()
        
        if side < (4 * 倍率): return
        self.forward(side)
        self.right(111)
        self.forward(side / 1.78)
        self.right(111)
        self.forward(side / 1.3)
        self.right(146)
        self.畫右三角(side * .75, 倍率,模式)

    def 畫左三角(self, side, 倍率,模式):
        if(模式==3): self.變化顏色()
        
        if side < (4 * 倍率): return
        self.forward(side)
        self.left(111)
        self.forward(side / 1.78)
        self.left(111)
        self.forward(side / 1.3)
        self.left(146)
        self.畫左三角(side * .75, 倍率,模式)

    def 中央區塊(self, s, a, 倍率):
        self.forward(s); self.left(a)
        if s < (7.5 * 倍率):
            return
        self.中央區塊(s - (1.2 * 倍率), a, 倍率)

def main():
    t = 設計圖()       
    t.speed(0)
    t.hideturtle()
    t.getscreen().delay(0)
    t.getscreen().tracer(0)
    
    at = clock()
    
    t.畫圖(t.position(), 1)   #顏色模式1(大部件隨機顏色)
    t.畫圖((-200,200), 1, 2)  #顏色模式2(大部件之小部件隨機顏色)
    t.畫圖((200,-200), 1, 3)  #顏色模式3(更細小部件隨機顏色)
    
    et = clock()
    
    return "執行時間: %.2f sec." % (et-at)

if __name__ == '__main__':
    msg = main()
    print(msg)
    mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
