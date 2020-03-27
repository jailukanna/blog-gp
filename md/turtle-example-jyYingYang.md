---
title: turtle example jyYingYang (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'jyYingYang'

Functions in program: 
* `def main():`
* `def yin(radius, color1, color2):   #radius半徑`

## python jyYingYang

Python turtle example: jyYingYang

```python
from turtle import * 

def yin(radius, color1, color2):   #radius半徑
    width(3)  #width寬度(turtle經過的路的寬度)
    color("red", color1) #turtle經過的路的顏色  color1==底色
    begin_fill() #開始填色
    circle(radius/2., 180) #半圓  
	#circle() 的()代表圓的大小  
	#2.=2.0 
	#circle(radius/2)是一整顆圓  
	#circle(radius/2 , 180)是半圓，因此circle(radius/2 , 90)應該是四分之一圓，以此類推
    circle(radius, 180) #同上，只是半圓比較大
    left(180)
    circle(-radius/2., 180) #-radius 的 - 是往下開始畫， 沒有-是往上開始畫
    end_fill()
	#以上完成左半邊(尚未畫裡面的小圓)
    left(90)
    up() #沒有痕跡 只有移動
    forward(radius*0.35) #forward向前 0.35 -->三分之一的估計值 此時中間小圓還是黑色
    right(90)
    down()#下筆，有痕跡+移動
    color(color1, color2) #此處的color1 == 邊框色彩 color2 ==底色
    begin_fill() #開始填塞
    circle(radius*0.15)
    end_fill()
    #整個左半邊完成包含小圓，接下來只是要回到原點而已
    left(90)
    up()
    backward(radius*0.35)
    down()
    left(90)

def main():
    reset() 
    yin(200, "black", "white")
    yin(200, "white", "black")
    ht()  #hide turtle
    return "Done!" #當main()執行完後，從def main()將資料傳出

if __name__ == '__main__':
    main()
    mainloop()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
