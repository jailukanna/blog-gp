---
title: turtle example Project Hitting Game (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Project Hitting Game'

Functions in program: 
* `def make_bubble(radius,delay,colour,x,y,score):`

Modules used in program: 
* `import math`

## python Project Hitting Game

Python turtle example: Project Hitting Game

```python
from turtle import *
import math
from random import randint
bubble=Turtle()
text=Turtle()
click_handler=Turtle()
game_begin=Turtle()
bubble.screen.bgcolor("black")
bubble_presence=False
text2=Turtle()
def make_bubble(radius,delay,colour,x,y,score):
    bubble_presence=True
    bubble.pencolor(colour)
    bubble.fillcolor(colour)
    i=radius
    a=0
    while i>1:
        delay=delay-a
        a=a+0.01
        i=i-0.3
        bubble.speed(0)
        bubble.penup()
        bubble.goto(x,y+100-i)
        bubble.hideturtle()
        bubble.pendown()
        bubble.speed(0)
        bubble._delay(delay)
        bubble.hideturtle()
        bubble.speed(0)
        bubble.clear()
        bubble.begin_fill()
        bubble.circle(i,360)
        bubble.end_fill()
        bubble.hideturtle()
        onscreenclick(click_handler.goto)
        click_xcor=click_handler.xcor()
        click_ycor=click_handler.ycor()
        if (xcor-2*i)<click_xcor:
            if click_xcor<(xcor):
                if (ycor-2*i)<click_ycor:
                    if click_ycor>(ycor):
                        print("test successful")
                        i=1
                        bubble_presence=False
                        def score_extent(radius,clickx,clicky,x,y):
                            score_to_be_added=math.ceil((1/math.fabs(radius-math.sqrt(math.pow((clickx-x),2)+math.pow((clicky-y),2))))*1000)
                            return score_to_be_added
                        score_to_be_added=score_extent(iradius,click_xcor,click_ycor,x,y)
                        score=score+score_to_be_added
    return score
i=0
delay=2
sum=0
b=0
b1=0
number_of_bubbles=0
while i==0:
    if bubble_presence==False:
        iradius=randint(65,100)
        xcor=randint(-370,370)
        ycor=randint(-220,220)
        number_of_bubbles=number_of_bubbles+1
        b=make_bubble(iradius,2,"#EBA35B",xcor,ycor,b)
        if b1==b:
            i=1
        else:
            if b-b1>200:
                text2.goto(text.screen.window_width()/2-250,text.screen.window_height()/2-150)
                text2.pencolor("white")
                text2.write("Jackpot", move=False, align="center", font=("Segoe UI", 100, "normal"))
                text2.clear()
            b1=b
        delay=delay-0.08
        text.hideturtle()
        text.color("white")
        text.clear()
        text.penup()
        text.goto(text.screen.window_width()/2-150,text.screen.window_height()/2-50)
        text.pendown()
        text.write("Score: "+str(b), move=False, align="left", font=("Segoe UI", 20, "normal"))
        print(b)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
