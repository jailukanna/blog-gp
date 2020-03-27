---
title: turtle example paint (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'paint'

Functions in program: 
* `def spiral():`
* `def oval():`
* `def pryamougolnik():`
* `def krug():`
* `def myright(x,y):`
* `def myleft(x,y):`
* `def pr(a,b):`
* `def kv(f):`

Modules used in program: 
* `import math`

## python paint

Python turtle example: paint

```python
from turtle import *
import math
class MyTurtle(Turtle):
 def rectangle(self,a,b):
  self.fd(a)
  self.lt(90)
  self.fd(b)
  self.lt(90)
  self.fd(a)
  self.lt(90)
  self.fd(b)
  self.lt(90)
  
 def spiral(self,r,n):
  xc=t.xcor()
  yc=t.ycor()   
  a=0
  rn=0
  xc=xc+math.cos(a)*rn
  yc=yc+math.sin(a)*rn
  self.pu()
  self.goto(xc,yc)
  self.pd()
  for i in xrange (0,n):
   while a<2*math.pi and rn<r:
    xc=xc+math.cos(a)*rn
    yc=yc+math.sin(a)*rn
    self.goto(xc,yc)
    a+=0.1
    rn+=0.1
   a=0
   
 def romb(self,x,y,z,g):
  self.pu()
  self.goto(z,y)
  self.pd()
  self.goto(x,g)
  self.goto(x-z,y)
  self.goto(x,y-g)
  self.goto(z,y)

 def paral(self,x,y,z,g,m,l,j,i):
  self.goto(x,y)
  self.goto(z,g)
  self.goto(m,l)
  self.goto(j,i)
  self.goto(x,y)
  
 def oval(self,r):
  xc=t.xcor()
  yc=t.ycor()
  a=0
  self.pu()
  self.goto((xc+r)*2,yc)
  self.pd()
  while a<2*math.pi+0.1:
   xc=xc+math.cos(a)*r*2
   yc=yc+math.sin(a)*r
   self.goto(xc,yc)
   a+=0.1

t=MyTurtle()
t.reset()

t.reset()
t.speed(1000)

def kv(f):
 for i in xrange(0,4):
  t.fd(f)
  t.rt(90)

def pr(a,b):
 for i in xrange(0,2):
  t.fd(a)
  t.rt(90)
  t.fd(b)
  t.rt(90) 

t.pu()
t.goto(-236,271)
t.pd()
t.color('black')
t.begin_fill()
pr(321,21)
t.end_fill()

list=['black','grey','lightgrey','white','red','brown','orange','yellow','lightgreen','green','darkgreen','lightblue','blue','darkblue','purple','pink']
x=-235
y=270
for i in range (0,16):
 t.pu()
 t.goto(x,y)
 t.pd()
 t.color('black')
 t.begin_fill()
 kv(20)
 t.color(list[i])
 t.pu()
 t.fd(1)
 t.pd()
 t.fd(20)
 t.end_fill()
 x=x+20
t.color('white')
t.fd(-1)
t.color('black')
t.fd(-1)

x1=-235
y1=270

xs=-235
ys=250

t.pu()
t.goto(xs,ys-5)
t.pd()
t.color('lightgrey')
t.begin_fill()
pr(20,60)
t.lt(90)
t.end_fill()
for i in range (0,3):
 t.pu()
 t.goto(xs,ys-25)
 t.pd()
 t.color('grey')
 kv(20)
 t.rt(90)
 t.fd(20)
 t.lt(90)
 ys=ys-20

xs=-235
ys=250

t.pu()
t.color('black')
t.goto(xs+10,ys-15)
t.pd()
t.begin_fill()
t.circle(1)
t.end_fill()
t.pu()
t.goto(xs+10,ys-40)
t.rt(90)
t.pd()
t.begin_fill()
t.circle(5)
t.end_fill()
t.pu()
t.goto(xs+10,ys-63)
t.pd()
t.begin_fill()
t.circle(8)
t.end_fill()

xs=-235
ys=185

t.up()
t.goto(xs,ys-5)
t.down()
t.color('lightgrey')
t.begin_fill()
pr(20,40)
t.lt(90)
t.end_fill()
for i in range (0,2):
 t.pu()
 t.goto(xs,ys-25)
 t.pd()
 t.color('grey')
 kv(20)
 t.rt(90)
 t.fd(20)
 t.lt(90)
 ys=ys-20
xs=-235
ys=185
t.pu()
t.goto(-234,179)
t.pd()
t.rt(90)
t.color('black')
kv(18)
t.pu()
t.goto(-234,159)
t.pd()
t.color('black')
t.begin_fill()
kv(18)
t.end_fill()

t.pu()
t.color('black')
t.goto(0,0)
t.pd()

xs=-235
ys=185

listsize=[20,10,3]
def myleft(x,y):
 if x1<=x<=(x1+20*16) and (y1-20)<=y<=y1:
   xc=t.xcor()
   yc=t.ycor()
   num=(x-x1)//20
   C=list[int(num)]
   print("PEN",C)
   t.pensize(3)
   t.pencolor(C)
   t.pu()
   t.goto(170,270)
   t.pd()
   kv(30)
   t.pensize(1)
   t.up()
   t.goto(xc,yc)
   t.down()   
 elif xs<=x<=xs+20 and ys<=y<=ys+60:
   num=(y-ys)//20
   S=listsize[int(num)]
   print(S)
   t.pensize(S)
 elif -235<=x<=-215 and 165<=y<=185 :
  t.begin_fill()
 elif -235<=x<=-215 and 145<=y<=165 :
  t.end_fill()
 else:  
  t.goto(x,y)
  xx=x
  yy=y

def myright(x,y):
 if x1<=x<=(x1+20*16) and (y1-20)<=y<=y1:
   xc=t.xcor()
   yc=t.ycor()
   num=(x-x1)//20
   CZ=list[int(num)]
   print("ZALIVKA",CZ)
   t.pensize(1)
   t.up()
   t.goto(172,268)
   t.down()
   t.begin_fill()
   t.color(CZ)
   kv(26)
   t.end_fill()
   t.up()
   t.goto(xc,yc)
   t.down()

def krug():
 t.circle(50)

def pryamougolnik():
 w=100
 h=40
 for i in range(0,2):
  t.fd(w)
  t.lt(90)
  t.fd(h)
  t.lt(90)

def oval():
 t.oval(10)

def spiral():
 t.spiral(10,5)

t.screen.onclick(myleft,1)
t.screen.onclick(myright,3)
t.ondrag(myleft,2)
t.screen.onkey(krug,'k')
t.screen.onkey(pryamougolnik,'p')
t.screen.onkey(oval,'o')
t.screen.onkey(spiral,'s')

t.screen.listen()

input()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
