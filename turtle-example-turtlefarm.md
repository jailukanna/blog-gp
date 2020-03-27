---
title: turtle example turtlefarm (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'turtlefarm'

Functions in program: 
* `def drawEnclosure():`

Modules used in program: 
* `import random`

## python turtlefarm

Python turtle example: turtlefarm

```python
# Detta är ett test av användning av Turtle-biblioteket
# i Python. Programmet låter ett valfritt antal sköldpaddor
# gå runt i en inhägnad, varpå de studsar vid kollision
# mot väggen.
#
# Några roliga strukturer är:
#
#  * Hastighetens representation i vektorform
#
#  * Hantering av riktningsändring i samband med
#    kollision av vägg
#
#  * Sköldpaddans vinkel följer färdriktningen
#    (här fick jag fundera på hur man skulle hantera att
#    att arctan-funktionen enbart returnerar vinklar i
#    intervallet -90° till 90°)
#
#  * Sköldpaddor skapas som kloner ur en klass, där varje
#    sköldpadda får sina egna egenskaper (hastighet, färg
#    och startposition)
#
# Copyleft: Nikodemus Karlsson

from turtle import *
from math import atan, pi
from random import *
import random

numberOfTurtles = 5

# Definition av ritområdet
myPlayground = Screen()
myPlayground.bgcolor("lightgreen")

# Inhägnadens hörn definieras
upperLeft = (-200, 200)
upperRight = (200, 200)
lowerRight = (200, -200)
lowerLeft = (-200, -200)

class TurtleWalk:
  def __init__(self, color):
    
    self.player = Turtle()
    self.color = color
    self.player.up()
    self.player.speed(0)
    self.player.shape("turtle")
    self.player.color(self.color)
    
    # Sköldpaddans startposition initieras
    # x- och y-läget mellan -200 och 200
    self.x, self.y = randint(-180, 180), randint(-180, 180)
    self.player.goto(self.x, self.y)

    # Sköldpaddans starthastighet initieras på vektorform
    # (slumpas som heltal mellan -10 och 10 i respektive
    # riktning, noll exkluderat).
    # För att funktionen atan senare ska ge tillfredsställande 
    # resultat måste värdena typomvandlas till flyttal.
    # (Om hastigheten i en riktning får värdet noll kommer
    # sköldpaddan enbart att röra sig utefter en linje.)
    acceptedVelocities = range(-10, 11)
    del acceptedVelocities[10]
    self.vx = random.choice(acceptedVelocities)
    self.vy = random.choice(acceptedVelocities)
    self.vx, self.vy = self.vx * 1.0, self.vy * 1.0
  
  def move(self):
    # Sköldpaddan förflyttas i inhägnaden,
    # initialt med starthastigheten storlek och riktning.
    while(1):
      # Sköldpaddan vrids i aktuell rörelseriktning
      # Eftersom atan-funktionen enbart ger en vinkel i området
      # -pi/2 < v < pi/2 så måste en koll ske om rörelseriktningen
      # är bakåt (vx < 0). I så fall vrids sköldpaddan 180 grader
      # för att det inte ska se ut som den färdas baklänges.
      if(self.vx > 0):
        self.player.setheading(180 * atan(self.vy / self.vx) / pi)
      else:
        self.player.setheading(180 * atan(self.vy / self.vx) / pi + 180)
    
      # Sköldpaddan flyttas i hastighetens riktning
      self.player.goto(self.player.pos()[0] + self.vx, self.player.pos()[1] + self.vy)
  
      # Studsa upp och ned
      if self.player.pos()[1] > 200 - 10 or self.player.pos()[1] < -200 + 10:
        self.vy = -self.vy
    
      # Studsa åt höger eller vänster
      if self.player.pos()[0] > 200 - 10 or self.player.pos()[0] < -200 + 10:
        self.vx = -self.vx
        
      return()
        
  def run(self):
    self.move()
    return()

# Funktion som ritar inhägnaden
def drawEnclosure():
  drawer = Turtle()
  drawer.speed(0)
  drawer.pensize(8)
  drawer.ht() # Göm sköldpaddan
  drawer.up() # Gör inget streck när sköldpaddan placeras
  drawer.goto(upperLeft)
  drawer.down()
  drawer.goto(upperRight)
  drawer.goto(lowerRight)
  drawer.goto(lowerLeft)
  drawer.goto(upperLeft)
  del drawer # Nu behövs inte "ritaren" längre

# ---------- Huvudprogram ----------

# Inhägnaden ritas
drawEnclosure()

# Färgerna på sköldpaddorna definieras
turtleColors = ["blue", "red", "yellow", "white", "black"]

# Definiera en array...
turtles = []

# ...att lägga sköldpaddorna i
for i in range(numberOfTurtles):
  aTurtle = TurtleWalk(turtleColors[i % len(turtleColors)])
  turtles.append(aTurtle)

# Respektive sköldpadda flyttas ett steg per loopvarv
while(1):
  for turtle in turtles:
    turtle.run()




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
