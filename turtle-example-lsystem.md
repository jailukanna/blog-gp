---
title: turtle example lsystem (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'lsystem'

Functions in program: 
* `def sortList(List):`
* `def HelperRecursiveSort(List,lo,hi):`
* `def lsystem(axiom,rules,depth,initialPosition,heading,angle,length):`
* `def drawLS(aTurtle,instructions,angle,distance):`
* `def applyProduction(axiom,rules,n):`

Modules used in program: 
* `import random`

## python lsystem

Python turtle example: lsystem

```python
from turtle import Turtle
import random

## Listing 9.6
# applyProduction: string dict of char:string integer -> string
def applyProduction(axiom,rules,n):
    for i in range(n):
        newString = ""
#        for ch in axiom:
#            newString = newString + rules.get(ch,ch)
#        axiom = newString
#    return axiom

        for ch in axiom:
            replVal = rules.get(ch,ch)
            if(type(replVal) is str):
                newString = newString + replVal
            elif(type(replVal) is list):
                rule = random.choice(replVal)
                newString = newString + rule
            else:
                newString = newString
        axiom = newString
    return axiom


## Listing 9.7
# drawLS: Turtle, string, float, float, -> void
def drawLS(aTurtle,instructions,angle,distance):
    stateSaver = []
    for cmd in instructions:
        if cmd == 'G':
            aTurtle.up();
            aTurtle.forward(distance);
            aTurtle.down();
        if cmd == 'F':
            aTurtle.forward(distance)
        elif cmd == 'B':
            aTurtle.backward(distance)
        elif cmd == '+':
            aTurtle.right(angle)
        elif cmd == '-':
            aTurtle.left(angle)
        elif cmd == '[':
            pos = aTurtle.position()
            head = aTurtle.heading()
            stateSaver.append((pos,head))
        elif cmd == ']':
            pos,head = stateSaver.pop()
            aTurtle.up()  
            aTurtle.setposition(pos)
            aTurtle.setheading(head)
            aTurtle.down() 

## Listing 9.8
# lsystem: string, dict, integer, tuple, float, float, float, -> void
def lsystem(axiom,rules,depth,initialPosition,heading,angle,length):
    aTurtle = Turtle()
    aTurtle.speed(10)       
    aTurtle.shape('blank') 
    aTurtle.up()
    aTurtle.setposition(initialPosition)
    aTurtle.down()
    aTurtle.setheading(heading)
    instructions = applyProduction(axiom,rules,depth)
    drawLS(aTurtle,instructions,angle,length)
    aTurtle.getscreen().exitonclick()


#Extra Credit:   
def HelperRecursiveSort(List,lo,hi):
    maxNum=List[lo];
    for num in List[lo:hi]:
        if (num>maxNum):
            maxNum=num;            
    if (List.index(maxNum)!=hi-1):
        maxIndex=List.index(maxNum)
        c=List[hi-1]
        List[hi-1]=maxNum
        List[maxIndex]=c
    if (hi>lo+1):
        HelperRecursiveSort(List,lo,hi-1)

def sortList(List):
    HelperRecursiveSort(List,0,len(List));
    return List;



#Run module before attempting to run an lsystem task

#task 1: Level 4 Koch curve with length unit of 6.
# lsystem('F',{'F':['F-F++F-F']},4,(-216,0),0,60,6)

 
#task 2: Support the 'G' operation.


#task 3: Draw the Sierpinski triangle:
# lsystem('F-F-F',{'F':['F-F-F-GG'],'G':['GG']},4,(0,0),0,120,6)


#task 4: 
# lsystem('F',{'F':['F[-F]F[+F]F']},4,(0,-150),90,25,6)


#task 5:
# lsystem('F',{'F':['F[-F]F[+F]F','F[-F]F','F[+F]F']},4,(0,0),90,25,6)


#Extra Credit:
# lsystem('F',{'F':['FF-[-F+F+F]+[+F-F-F]']},4,(-9,0),90,22.5,6)
# lsystem('F',{'F':['F[+F[+F][-F]F][-F[+F][-F]F]F[+F][-F]F']},4,(-9,0),90,30,6)
    




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
