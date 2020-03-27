---
title: turtle example DenizAltiKomutani (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'DenizAltiKomutani'

Functions in program: 
* `def recycle_torpedo():`
* `def recycle_ship():`
* `def check_wolf_limits():`
* `def check_lives():`
* `def check_points():`
* `def hittrade(A):`
* `def hithospital(A):`
* `def hitbattle():`
* `def check_torpedos():`
* `def check():`
* `def show():`
* `def gohospital():`
* `def gotradeship():`
* `def gobattleship():`
* `def hospital():`
* `def trade():`
* `def battle():`
* `def move_computer():`
* `def carry_torpedos(): `
* `def move_wolf_shoot():`
* `def move_wolf_right():`
* `def move_wolf_left():`
* `def move_wolf_down():`
* `def move_wolf_up():`
* `def move_player(playing):                                   ###`
* `def move (playing):`
* `def plate():`
* `def crazy_rabbit():`
* `def give_playing():`

Modules used in program: 
* `import time`
* `import random`
* `import turtle`

## python DenizAltiKomutani

Python turtle example: DenizAltiKomutani

```python
# copyright : talha büyükakkaşlar
# -*- coding: cp1254 -*-
import turtle
from turtle import *
import random
import time
# bu kısım bize istediğimiz şekilde turtle yapma imkanı sağlayacak
turtle.register_shape("ship",((4,12),(4,2),(8,2),(8,6),(4,6),(4,-4),(0,-4),(0,8)))
turtle.register_shape("sub",((-4,10),(-4,4),(-8,4),(-8,0),(-4,0),(-4,-10),(0,-10),(0,10),(-2,12)))
turtle.register_shape("torpedo",((0,2),(-10,2),(-10,0),(0,0)))
t=turtle
class ships():
    def __init__(self,model):
        self.body=turtle.clone()             
        self.body.speed(0)
        self.body.penup()
        self.body.hideturtle()
        self.body.setpos(500,150)
        self.body.right(180)
        self.taskx=0
        self.tasky=0
        self.isalive=False
        if model=="hood":
            self.body.shape("ship")
            self.body.color("red")
            self.speedy=0
            self.speedx=1
        elif model=="carpathia":
            self.body.shape("ship")
            self.body.color("darkgreen")
            self.speedy=0
            self.speedx=1
        elif model=="atlantic" :
            self.body.shape("ship")
            self.body.color("yellow")
            self.speedy=0
            self.speedx=2
        elif model=="falke":
            self.body.shape("torpedo")
            self.body.color("black")
            self.speedx=0
            self.speedy=10     
            self.body.setpos(0,0)
            self.body.right(180)
        elif model == "wolf":
            self.body.shape("sub")
            self.body.color("black")
            self.speedy=3
            self.speedx=4
            self.body.setpos(0,0)
            self.body.right(180)
        elif model == "globe":
            self.lives=0
            self.Points=0
            self.game=True
        else:
            print("hata model")
def give_playing():
    playing=wolf[0]
    for i in range (len(wolf)):
        if wolf[i].isalive:
            return wolf[i]
########Tavşan başa############
def crazy_rabbit():
    if rabbit.body.xcor() >= 500:
        rabbit.taskx=-2000
    if rabbit.body.xcor() <= -500:
        rabbit.taskx=2000
##############################en dış fonksyon plate
def plate():
    oyun.game=True
    while oyun.game:
        playing = give_playing()
        move(playing)
        show()
        check()
        crazy_rabbit()
        time.sleep(0.04)
######################## plate'nin parçası move
def move (playing):
    move_player(playing)
    move_computer()
##############move'nin parçası move_player#####################
def move_player(playing):                                   ###
    playing.body.screen.onkey(move_wolf_up,"Up")            ###
    playing.body.screen.onkey(move_wolf_down,"Down")        ###
    playing.body.screen.onkey(move_wolf_left,"Left")        ###
    playing.body.screen.onkey(move_wolf_right,"Right")      ###
    playing.body.screen.onkey(move_wolf_shoot,"space")      ###
    playing.body.screen.listen()                            ###
    carry_torpedos()                                        ###
#######move_player' in parçaları artık en dipteyiz###       ###
def move_wolf_up():
    playing = give_playing()
    playing.tasky +=20
def move_wolf_down():
    playing = give_playing()
    playing.tasky -=20
def move_wolf_left():
    playing = give_playing()
    playing.taskx -=20
def move_wolf_right():
    playing = give_playing()
    playing.taskx +=20
def move_wolf_shoot():
    playing = give_playing()
    for i in range (len(falke)):
        if not falke[i].isalive:
            falke[i].isalive = True
            falke[i].tasky=1000
            falke[i].body.showturtle()
            break             #
def carry_torpedos(): 
    playing = give_playing()                              #
    for j in range (len(falke)-1):
        if not falke[j].isalive:
            falke[j].body.setposition(playing.body.xcor(),playing.body.ycor())


##############move'nin parçası move_computer#########################
def move_computer():
    battle()
    trade()
    hospital()
##############move_computerin parçalari#########
def battle():
    x = random.randint(0,40)
    if x>=1:
        x=0
    else:
        x=1
    if bool(x):
        gobattleship()
def trade():
    x = random.randint(0,10)
    if x>=1:
        x=0
    else:
        x=1
    if bool(x):
        gotradeship()
def hospital():
    x = random.randint(0,50)
    if x>=1:
        x=0
    else:
        x=1
    if bool(x):
        gohospital()
def gobattleship():
    for i in range(len(hood)):
        if not hood[i].isalive:
            hood[i].isalive=True
            hood[i].body.showturtle()
            hood[i].taskx=-10000
            break
def gotradeship():
    for i in range(len(atlantic)):
        if not atlantic[i].isalive:
            atlantic[i].isalive=True
            atlantic[i].body.showturtle()
            atlantic[i].taskx=-10000
            break
def gohospital():
    for i in range(len(carpathia)):
        if not carpathia[i].isalive:
            carpathia[i].isalive=True
            carpathia[i].body.showturtle()
            carpathia[i].taskx=-10000
            break
##############plate'nin parçası show#####################
def show():
    for i in range(len(tamliste)):
        for j in range(len(tamliste[i])):
            if tamliste[i][j].isalive:
                if tamliste[i][j].taskx>tamliste[i][j].speedx :
                    tamliste[i][j].body.setpos((tamliste[i][j].body.xcor()+tamliste[i][j].speedx),tamliste[i][j].body.ycor())
                    tamliste[i][j].taskx -= tamliste[i][j].speedx
                if tamliste[i][j].tasky>tamliste[i][j].speedy:
                    tamliste[i][j].body.setpos(tamliste[i][j].body.xcor(),tamliste[i][j].body.ycor()+tamliste[i][j].speedy)
                    tamliste[i][j].tasky -= tamliste[i][j].speedy
                if tamliste[i][j].taskx<0 :
                    tamliste[i][j].body.setpos((tamliste[i][j].body.xcor()-tamliste[i][j].speedx),tamliste[i][j].body.ycor())
                    tamliste[i][j].taskx += tamliste[i][j].speedx
                if tamliste[i][j].tasky<0:
                    tamliste[i][j].body.setpos(tamliste[i][j].body.xcor(),tamliste[i][j].body.ycor()-tamliste[i][j].speedy)
                    tamliste[i][j].tasky += tamliste[i][j].speedy
##############plate'nin parçası check#####################
def check():
    check_torpedos()
    check_points()
    check_lives()
    check_wolf_limits()
    recycle_ship()
    recycle_torpedo()

##############check'nin parçası check_torpedos#####################
def check_torpedos():
    for k in range(len(falke)-1):
        if (falke[k].body.ycor()<=160) and (falke[k].body.ycor()>=140):
            for i in range(len(tamliste)):
                for j in range(len(tamliste[i])):
                    if tamliste[i][j].isalive and (((falke[k].body.xcor() >= tamliste[i][j].body.xcor()-15)) and ((falke[k].body.xcor() <=tamliste[i][j].body.xcor()+15))):
                        if tamliste[i][j].body.color()==atlantic[0].body.color():
                            hittrade(tamliste[i][j])
                        elif tamliste[i][j].body.color()==hood[0].body.color():
                            hitbattle()
                        elif tamliste[i][j].body.color()==carpathia[0].body.color():
                            hithospital(tamliste[i][j])
                        elif tamliste[i][j].body.color()==wolf[0].body.color():
                            bossayi=0
                        else:
                            print(tamliste[i][j].body.color())
                            print("hata check_torpedos")
##############check_torpedos'nun parçaları#####################
def hitbattle():
    print("savaş gemilerine zarar veremezsin")
def hithospital(A):
    oyun.lives=3
    A.isalive=False
    A.taskx=0
    A.tasky=0
    A.body.hideturtle()
    A.body.setposition(500,150)
    print"kimse hastahane gemisini vurmaz oyunu kaybettin"
def hittrade(A):
    A.isalive=False
    oyun.Points+=1
    A.taskx=0
    A.tasky=0
    A.body.hideturtle()
    A.body.setposition(500,150)
##############check'nin parçaları#####################
def check_points():
    if oyun.Points==10:
        print("oyunu kazandın")
        oyun.game=False
def check_lives():
    if oyun.lives==3:
        print("oyunu kaybettin")
        oyun.game=False
def check_wolf_limits():
    playing = give_playing()
    if (playing.body.xcor()>500):
        playing.taskx = -10
    elif(playing.body.xcor()<-500):
        playing.taskx = 10
    if (playing.body.ycor()>150):
        playing.tasky = -10
    elif (playing.body.ycor<-250):
        playing.tasky = 10
def recycle_ship():
    for i in range(2,len(tamliste)):
        for j in range(2,len(tamliste[i])):
            if (tamliste[i][j].body.xcor()<-500):
                tamliste[i][j].isalive=False
                tamliste[i][j].body.hideturtle()
                tamliste[i][j].body.setposition(500,150)
def recycle_torpedo():
    for i in range(len(falke)):
        if falke[i].isalive:
            if falke[i].body.ycor()>500:
                falke[i].isalive=False
                falke[i].body.hideturtle()


########Gösterim kısımı  C dilndeki Main Fonksyonu#####################
oyun = ships("globe")
t.tracer(3)
# Falke ikinci dünya savaşı alman torpidosu
falke = [0,1,2,3,4,]
for i in range(4):
    falke[i]=ships("falke")
# Wolf (kurt) ikinci dünya savaşında alman deniz altıların lakabıdır
wolf = [0,1,2,]
for i in range(3):
    wolf[i]=ships("wolf")
# Atlantic şimdiye kadar inşa edilmiş en büyük 3. gemidir. Ticari petrol tankeridir
atlantic = [0,1,2,3,4,5,6,7,8,9,]
for i in range(10):
    atlantic[i]=ships("atlantic")
# Hood İngiliz donanmasının sahip olduğu büyük savaş gemisi 2. dünya savaşında batırıldı
hood = [0,1,2,3,4,]
for i in range(5):
    hood[i]=ships("hood")
# Carpatihia bir yolcu gemisi Titanic yolcularını kurtarması ile tanınır ayrıca 2. dünya savaşında hastahane gemisi iken batmıştır.
carpathia = [0,1,2,3,4,]
for i in range(5):
    carpathia[i]=ships("carpathia")
tamliste=[falke,wolf,atlantic,hood,carpathia]
t.setup(width=1000,height=500,startx=100,starty=100)
t.penup()
t.setpos(500,150)
t.pendown()
t.begin_fill()
t.fillcolor("turquoise")
t.right(180)
t.forward(1000)
t.right(90)
t.forward(100)
t.right(90)
t.forward(1000)
t.right(90)
t.forward(100)
t.begin_fill()
t.fillcolor("blue")
t.forward(400)
t.right(90)
t.forward(1000)
t.right(90)
t.forward(400)
t.right(90)
t.forward(1000)
t.end_fill()
falke[4] = ships("falke")
rabbit = falke[4]
rabbit.isalive=True
rabbit.speedx=10
rabbit.speedy=10
rabbit.taskx=1000

wolf[0].isalive = True
wolf[0].body.showturtle()
plate()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
