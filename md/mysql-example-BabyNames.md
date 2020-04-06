---
title: mysql example BabyNames (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'BabyNames'

Functions in program: 
* `def main():`
* `def WriteName(screen, nameD, myfont, color, number):`
* `def ClosestName3(closestnames, string):`
* `def ClosestName2(closestnames, string):`
* `def ClosestName1(closestnames, string):`
* `def Search3(theNames, string):`
* `def SearchName(theNames, string):`
* `def draw(screen, theNames, myFont, nameIndx):`
* `def readData(theNames):`

Modules used in program: 
* `import sys, pygame`
* `import random`
* `import math`

## python BabyNames

Python mysql example: BabyNames

```python
#Lab 7 for cpe 101 - reads in a text file of names and their popularity over
# the last 100 years and plots the popularity for a given name
# the user may choose to search for a name at the command prompt

import math
import random
import sys, pygame
from pygame.locals import *

# define some colors to be used for drawing
PURPLE = (240, 5, 213)
AQUA = (64, 255, 244)
AQUAG = (84, 204, 163)
BLUE = (84, 204, 255)
PBLUE = (61, 115, 255)
SEAGREEN = (44, 232, 140)
CELL = 400//11

#name data - has a name string and the popularity of that name
class nData:
   def __init__(self, in_name, in_rank) :
      self.name = in_name
      self.rankList = in_rank

#function to read in the text file and create a giant list of name data elements
def readData(theNames):
   f = open('names.txt', 'r')
   j = 0
   ranks = []

   while True:
      s = f.readline()
      if len(s) == 0:
         break
      #split the line apart
      l = s.split()
      #put each year's popularity into a list (not integars yet)
      for i in range(1, len(l)):
         ranks.append(l[i])
      #add the new name and its ranking over time
      theNames.append(nData(l[0], ranks))
      ranks = []
      j += 1

#currently draws a graph representing the popularity of the name at the
#specified index over time with ellipses and lines in between
def draw(screen, theNames, myFont, nameIndx):
   colors = [SEAGREEN, AQUA, BLUE, PBLUE, PURPLE]
   screen.fill((128, 128, 128))

   shiftUp = 10
   
   for n in range(0,len(nameIndx)):
      WriteName(screen, theNames[nameIndx[n]], myFont, colors[n], n+1)
      for i in range(0, 11):
         CurY = (screen.get_height() - 
                  int(theNames[nameIndx[n]].rankList[i])*0.4 - shiftUp)
         pygame.draw.ellipse(screen, colors[n], Rect(i*CELL, CurY, 16, 16))
         #only draw lines between ellipses
         if i < 10:
            NextY = (screen.get_height() -
                      int(theNames[nameIndx[n]].rankList[i+1])*0.4 -shiftUp)
            pygame.draw.line(screen, colors[n], (i*CELL+8, CurY+8), 
                                       ((i+1)*CELL+8, NextY+8), 2)


#looks through all the names for a particular one
def SearchName(theNames, string):
   for i in range(0, len(theNames)):
      listnames = theNames[i].split[0:3]
      if string == theNames[i].name:
         return i
      elif string == listnames:
         return i
   return -1

def Search3(theNames, string):
   for i in range(0, len(theNames)):
      new_string = string.split[0:3]
      listnames = theNames[i].split[0:3]
      if new_string == listnames:
         return i
   return -1

def ClosestName1(closestnames, string):
   for i in range(len(string)):
      if string[i] == "A":
         string = string.replace(string[i], "E")
         closestnames.append(string)
      elif string[i] == "a":
         string = string.replace(string[i], "e")
         closestnames.append(string)

def ClosestName2(closestnames, string):
   for i in range(len(string)):
      if string[i] == "E":
         string = string.replace(string[i], "A")
         closestnames.append(string)
      elif string[i] == "e":
         string = string.replace(string[i], "a")
         closestnames.append(string)

def ClosestName3(closestnames, string):
   for i in range(len(string)):
      if string[i] == "y":
         string = string.replace(string[i], "ie")
         closestnames.append(string)


         
#writes out the name being graphed
def WriteName(screen, nameD, myfont, color, number):
   label = myfont.render("Name: " + str(nameD.name), 1, color)
   screen.blit(label, (10, 10*number))

#function to start up the main drawing
def main():

   pygame.init()
   width = 400
   height = 480
   screen = pygame.display.set_mode((width, height))
   theNames = []
   closestnames = []
   readData(theNames)
   nameIndx = []

   #get my font ready for when I do print(to the screen)
   myfont = pygame.font.SysFont("monospace", 15)

   while True:
      for event in pygame.event.get():
         if event.type == QUIT: sys.exit()

      draw(screen, theNames, myfont, nameIndx)
      pygame.display.flip()
      #python3 no longer supports raw_input but lower versions do
      #var = raw_input("Enter a name to search for: ")
      var = input("Enter a name to search for: ")
      nameIndx = []
      closestnames = []
      print("You entered " + str(var))
      ClosestName1(closestnames, str(var))
      ClosestName2(closestnames, str(var))
      ClosestName3(closestnames, str(var))
      closestnames.append(str(var).split[0:3])
      print(closestnames)
      curI = SearchName(theNames, str(var))
      if curI >= 0:
         nameIndx.append(curI)
         for i in range(0,len(closestnames)):
            newI = SearchName(theNames, closestnames[i])
            if newI >=0:
               nameIndx.append(newI)
      else:
         print("Name not found using default name A")
         for index in range(0,3):
            nameIndx.append(0)
         

if __name__ == '__main__':
   main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
