---
title: turtle example HW1 2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'HW1 2'

Functions in program: 
* `def main():`

## python HW1 2

Python turtle example: HW1 2

```python
from turtle import *
def main():
    reset()
    bgcolor("black")
	#身體小圓
    up()
    goto(0,40)
    radius=150
    width(5)
    color("skyblue")
    begin_fill()
    circle(70, 360)
    end_fill()
	
	#身體大圓
    goto(0,-140)
    width(5)
    color("skyblue")
    begin_fill()
    circle(100, 360)
    end_fill()

	#左眼眼白
    goto(-25,110)
    width(1)
    begin_fill()
    down()
    color("black","white")
    circle(15, 360)
    up()
    end_fill()
    
	#左眼眼珠
    goto(-25,118)
    width(1)
    color("black")
    begin_fill()
    circle(6, 360)
    end_fill()
	
	#右眼眼白
    goto(25,110)
    width(1)
    begin_fill()
    down()
    color("black","white")
    circle(15, 360)
    up()
    end_fill()
	
	#右眼眼珠
    goto(25,118)
    width(1)
    color("black")
    begin_fill()
    circle(6, 360)
    end_fill()
	
	#鼻子
    goto(0,110)
    width(1)
    begin_fill()
    color("orange")
    down()
    right(60)
    forward(12)
    left(60)
    backward(12)
    left(60)
    forward(12)
    up()
    end_fill()
    right(60)

	#嘴巴
    goto(-20,85)
    right(90)
    width(1)
    begin_fill()
    down()
    color("black","red")
    circle(20, 180)
    right(90)
    backward(40)
    up()
    end_fill()
    left(90)

	#鈕扣1
    goto(10,20)
    width(1)
    color("royalblue")
    begin_fill()
    circle(10, 360)
    end_fill()

	#鈕扣2
    goto(10,-15)
    width(1)
    color("royalblue")
    begin_fill()
    circle(10, 360)
    end_fill()

	#鈕扣3
    goto(10,-50)
    width(1)
    color("royalblue")
    begin_fill()
    circle(10, 360)
    end_fill()
	
	#鈕扣4
    goto(10,-85)
    width(1)
    color("royalblue")
    begin_fill()
    circle(10, 360)
    end_fill()

	#右手
    goto(85,0)
    width(8)
    right(60)
    color("saddlebrown")
    down()
    forward(100)
    up()
    backward(30)
    left(40)
    down()
    forward(30)
    up()
    backward(30)
    right(80)
    down()
    forward(30)
    up()
    left(100)

	#左手
    goto(-85,0)
    width(8)
    left(60)
    color("saddlebrown")
    down()
    forward(100)
    up()
    backward(30)
    left(40)
    down()
    forward(30)
    up()
    backward(30)
    right(80)
    down()
    forward(30)
    up()
    right(20)

	#頭髮
    goto(-30,170)
    width(5)
    left(40)
    color("yellow")
    down()
    forward(35)
    up()
    goto(0,180)
    width(5)
    right(40)
    color("yellow")
    down()
    forward(35)
    up()
    goto(30,170)
    width(5)
    right(40)
    color("yellow")
    down()
    forward(35)
    up()
    left(40)
    
	#H
    goto(100,160)
    width(5)
    color("blue")
    right(180)
    down()
    forward(50)
    up()
    goto(100,135)
    left(90)
    down()
    forward(35)
    up()
    goto(135,160)
    right(90)
    down()
    forward(50)
    up()
    
	#E
    goto(145,160)
    down()
    forward(50)
    up()
    goto(145,160)
    left(90)
    down()
    forward(35)
    up()
    goto(145,135)
    down()
    forward(35)
    up()
    goto(145,110)
    down()
    forward(35)
    up()
    
	#L
    goto(190,160)
    right(90)
    down()
    forward(50)
    up()
    goto(190,110)
    left(90)
    down()
    forward(35)
    up()

	#L
    goto(235,160)
    right(90)
    down()
    forward(50)
    up()
    goto(235,110)
    left(90)
    down()
    forward(35)
    up()

	#O
    goto(275,135)
    right(90)
    down()
    circle(28, 360)
    up() 
    return "Done!"
if __name__ == '__main__':
    main()
    mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
