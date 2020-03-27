---
title: turtle example ryYinyangRotation en (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'ryYinyangRotation en'

Functions in program: 
* `def yinyang():`
* `def yin(radius, color1, color2):`
* `def makeYinyangShape(ratio= 10, yinColor= 'black', yangColor= 'white'):`
* `def yinyangRotation01():`
* `def yinyangRotation02():`
* `def yinyangRotation03():`
* `def yinyangRotation04():`
* `def yinyangRotation05():`
* `def main():`

## python ryYinyangRotation en

Python turtle example: ryYinyangRotation en

```python
'''
ryYinyangRotation_en.py
 
After playing with yinyang.py in Turtle Graphics,
we will make it rotate.
 
use sin and cos from math module 
for rotation computation
 
Renyuan
2015/03/15
 
'''
 
from turtle  import *
from math    import sin, cos
from cmath   import exp
 
T= 100 # the total time for rotation
 
def main():
 
     
    #yinyang()
    #clearscreen()
 
    '''
    makeYinyangShape()
    clearscreen()
     
    yinyangRotation01()
    clearscreen()
     
    yinyangRotation02()
    clearscreen()
     
    yinyangRotation03()
    clearscreen()
     
    yinyangRotation04()
    clearscreen()
    '''   
    yinyangRotation05()
     
    return 'Done!'
 
 
def yinyangRotation05():
    '''
    rotate around a center,
    using different contour,
    using complex number in a polar form
    '''
 
    aTurtle= Turtle()
    aTurtle.stamp()
 
    T=1000  
    for t in range(T): 
 
        q= t/100 # q is used as theta, 角度
 
        # 運用 直角坐標
        #x= 200 * cos(q) #t)
        #y= 200 * sin(q) #t)
 
        # 轉成 複數 極座標，數學形式可能變簡單。
 
        #r= 100 # 圓
 
        #r= 100 *1 /(1 + .5*cos(q)) # 橢圓
 
        #r= 1 *(1 +q +q**2) # 螺線
 
        #r= 100*(1 +cos(q) +cos(2*q)) # 複合曲線
         
        r= 50*(sum([cos(k*q) for k in range(10)])) # 複合曲線
 
        z= r*exp(1j*q)
         
        x, y = z.real, z.imag
         
        aTurtle.goto(x,y)
             
def yinyangRotation04():
    '''
    a Big yinyang and a small yinyang, 
    self-rotate (spin), and rotate around a center.
    '''
 
    aTurtle= Turtle()
    aTurtle.shape('yinyang')
    aTurtle.shapesize(0.1)
     
    bTurtle= Turtle()
    bTurtle.shape('antiyinyang')
    bTurtle.shapesize(0.02)
             
    aTurtle.getscreen().bgcolor('grey')
    aTurtle.getscreen().tracer(1,10) # control the screen update
        
    for t in range(T): 
        aTurtle.right(10)
        q= t/100
        x= 200 * cos(q)
        y= 200 * sin(q)
        aTurtle.goto(x,y)
         
        bTurtle.left(50)
        q1= t/100 *5
        x1= x + 50 * cos(q1)
        y1= y + 50 * sin(q1)
        bTurtle.goto(x1,y1)
 
def yinyangRotation03():
    '''
    yinyang self-rotate and rotate around a center.
    '''
 
    aTurtle= Turtle()
    aTurtle.shape('yinyang')
    aTurtle.shapesize(0.2)
             
    aTurtle.getscreen().bgcolor('grey')
        
    for t in range(T): 
        aTurtle.right(10)
        q= t/100
        x= 200 * cos(q) #t)
        y= 200 * sin(q) #t)
        aTurtle.goto(x,y)
         
def yinyangRotation02():
    '''
    2 yinyangs，rotate clock-wise and counter clock-wise。
    '''
 
    aTurtle= Turtle()
    aTurtle.shape('yinyang')
         
    bTurtle= Turtle()
    bTurtle.shape('antiyinyang')
     
    aTurtle.goto( 200,0)
    bTurtle.goto(-200,0)
     
    aTurtle.getscreen().bgcolor('grey')
        
    for t in range(T): 
        aTurtle.right(1)
        bTurtle.left(1)
 
def yinyangRotation01():
    '''
    make a yinyang self-rotate.
    '''
 
    aTurtle= Turtle()
    aTurtle.shape('yinyang')
             
    aTurtle.getscreen().bgcolor('grey')
        
    for t in range(T): 
        aTurtle.right(1)
 
def makeYinyangShape(ratio= 10, yinColor= 'black', yangColor= 'white'):
    '''
    learn from yinyang.py
    plot it, get coordinates as a shape.
     
    shapeName is set as 
    'yinyang' and 'antiyinyang' 
    (symmetric left and right)
     
    main purpose is to make it rotate.
    '''
     
    shapeName= 'yinyang'
     
    #
    # learn from yinyang.py, plot it
    # The major point is to record its trail
    # save it as a polygon's coordinates list
    # m1, m2, m3, m4
    # are bigYin, bigYang, smallYang, smallYin, respectively.
    #
    aTurtle= Turtle()
 
    aTurtle.reset()
    aTurtle.speed(0) # aTurtle moves fastest
    aTurtle.getscreen().tracer(0,0) # the screen update
     
    aTurtle.setheading(0)
    aTurtle.begin_poly()
    aTurtle.circle(10*ratio, 180)
    aTurtle.circle(10*2*ratio, 180)
    aTurtle.left(180)
    aTurtle.circle(-10*ratio,  180)
    aTurtle.end_poly()
    m1= aTurtle.get_poly()
     
    aTurtle.setheading(180)
     
    aTurtle.begin_poly()
    aTurtle.circle(10*ratio, 180)
    aTurtle.circle(10*2*ratio, 180)
    aTurtle.left(180)
    aTurtle.circle(-10*ratio,  180)
    aTurtle.end_poly()
    m2= aTurtle.get_poly()
     
    aTurtle.setheading(90)
    aTurtle.forward(10/2*ratio)
    aTurtle.right(90)
     
    aTurtle.begin_poly()
    aTurtle.circle(10/2*ratio)
    aTurtle.end_poly()
    m3= aTurtle.get_poly()
     
    aTurtle.setheading(-90)
    aTurtle.forward(10/2*2*ratio)
    aTurtle.right(90)
     
    aTurtle.begin_poly()
    aTurtle.circle(10/2*ratio)
    aTurtle.end_poly()
    m4= aTurtle.get_poly()
     
    #
    # register this shape, name it as 'yinyang'
    #
    thisShape= Shape("compound")
    thisShape.addcomponent(m1,yinColor)
    thisShape.addcomponent(m2,yangColor)
    thisShape.addcomponent(m3,yangColor)
    thisShape.addcomponent(m4,yinColor)
     
    thisName= shapeName
    aTurtle.getscreen().register_shape(thisName, thisShape)
     
    #
    # do some computation, make m1, m2, symmetry left-right
    # register another shape, name it as 'antiyinyang'。
    # print('m1=', m1)
    #
    m10= []
    for (x,y) in m1:
        m10 += [(-x,y)]
    m20= []
    for (x,y) in m2:
        m20 += [(-x,y)]
     
    thatShape= Shape("compound")
    thatShape.addcomponent(m10,yinColor)
    thatShape.addcomponent(m20,yangColor)
    thatShape.addcomponent(m3,yangColor)
    thatShape.addcomponent(m4,yinColor)
     
    thatName= 'anti' + shapeName
    aTurtle.getscreen().register_shape(thatName, thatShape)    
     
    #
    # clear the screen.
    #
    aTurtle.home()
    aTurtle.clear()
    #aTurtle.ht()
    return
 
#
# from the original yinyang.py
# copy the following codes
#
def yin(radius, color1, color2):
    width(3)
    color("black", color1)
    begin_fill()
    circle(radius/2., 180)
    circle(radius, 180)
    left(180)
    circle(-radius/2., 180)
    end_fill()
    left(90)
    up()
    forward(radius*0.35)
    right(90)
    down()
    color(color1, color2)
    begin_fill()
    circle(radius*0.15)
    end_fill()
    left(90)
    up()
    backward(radius*0.35)
    down()
    left(90)
 
#
# rename the main() function in yinyang.py as "yinyang()"
#
def yinyang():
    reset()
    yin(200, "black", "white")
    yin(200, "white", "black")
    ht()
    return "Done!"
#
# the program really runs from here
#
if __name__=='__main__':
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
