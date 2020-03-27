---
title: turtle example turtlerace (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtlerace'


## python turtlerace

Python turtle example: turtlerace

```python
from turtle import *
from random import randint
from  Tkinter import *
class MyApp(Tk):
  def __init__(self):
    Tk.__init__(self)
    fr=Frame(self)
    self.player_button=Button(self, text="CLICK TO\nSTART RACE",command=self.do_exit,font=("Times New Roman",16))
    fr.grid()
    self.player_button.grid(row=1,column=1)
  def do_exit(self):
    root.destroy()

if __name__=='__main__':
  root=MyApp()
  root.title("Turtle Race")
  root.mainloop()
title("*****GAME:  TURTLE RACE*****")
bgcolor("black")
pencolor("white")
write("TuRtLe RaCe",align='left', font=("ravie",50, "normal",'italic','bold'))
pensize(5)
fillcolor("red")
for step in range(29):
  write(step, align='center')
  speed(111)
  right(90)
  forward(10)
  pendown()
  forward(150)
  penup()
  backward(160)
  left(90)
  forward(20)


delay(30)


ada1=Turtle()
speed(100)
ada1.color('brown')
ada1.shape('turtle')
ada1.penup()
ada1.goto(-12,-160)
ada1.pendown()
forward(20)
ada=Turtle()
ada.color('red')
ada.shape('turtle')
ada.penup()
ada.goto(-12,-140)
ada.pendown()
forward(30)
bob=Turtle()
speed(15)
bob.color('blue')
bob.shape('turtle')
bob.penup()
bob.goto(-12,-120)
bob.pendown()
forward(40)
bob1=Turtle()
bob1.color('yellow')
bob1.shape('turtle')
bob1.penup()
bob1.goto(-12,-100)
bob1.pendown()
forward(10)
bob2=Turtle()
speed(-10)
bob2.color('orange')
bob2.shape('turtle')
bob2.penup()
bob2.goto(-12,-80)
bob2.pendown()
bob3=Turtle()
speed(5)
bob3.color('violet')
bob3.shape('turtle')
bob3.penup()
bob3.goto(-12,-60)
bob3.pendown()
bob4=Turtle()
speed(30)
bob4.color('darkgreen')
bob4.shape('turtle')
bob4.penup()
bob4.goto(-12,-40)
bob4.pendown()
bob5=Turtle()
speed(40)
bob5.color('magenta')
bob5.shape('turtle')
bob5.penup()
bob5.goto(-12,-20)
bob5.pendown()



for turn in range(100):
  ada.forward(randint(1,10))
  bob.forward(randint(1,10))
  bob1.forward(randint(1,10))
  bob2.forward(randint(1,10))
  bob3.forward(randint(1,10))
  bob4.forward(randint(1,10))
  bob5.forward(randint(1,10))
  ada1.forward(randint(1,10))

write("FINISH",font=("ravie",15, "normal"),align='left',)
  
 


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
