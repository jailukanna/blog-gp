---
title: turtle example textpiecharrt (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'textpiecharrt'


Modules used in program: 
* `import math`
* `import string`

## python textpiecharrt

Python turtle example: textpiecharrt

```python
import string
import math
from turtle import Turtle

#all the intializers and declaration
#LettresLC has empty at last index which counts the whitespace
LettersLC= 'abcdefghijklmnopqrstuvwxyz@&%^:;.,=!-#*)(0123456789 '
Symbols_and_Numbers='@&%^:;.,=-!#*)(0123456789'
LettersUC='ABCDEFGHIJKLMNOPQRSTUVWXYZ'
Letters={}
probablity_of_each={}
total_letters=0
symbols=0
remaining =0
COLORS = ['yellow', 'violet','pink','green', 'red','gray', 'cyan', 'peru','black', 'blue', 'mediumpurple','white']
RADIUS = 175
LABEL_RADIUS = RADIUS * 1.1
FONTSIZE = 14
FONT = ("Ariel", FONTSIZE)

#creates and empty dicitionary having all the letters, symbols and numerals
for i in range(len(LettersLC)):
    Letters[LettersLC[i]]=0
#reading the file
fileContent=open("filename.txt",'r')
#loop that goes through all the lines and adds if a letter is repeated using string indexing
while(True):
    readContents=fileContent.readline()
    for i in range(len(readContents)):
        for j in range(len(LettersLC)):
            if(readContents[i]== LettersLC[j] ):
                Letters[LettersLC[j]]=Letters[LettersLC[j]]+1
        for k in range(len(LettersUC)):
            if(readContents[i]== LettersUC[k]):
               Letters[LettersLC[k]]=Letters[LettersLC[k]]+1
               
    if(readContents==""):
        break
#closes the file so that all the command can be executed
fileContent.close()

#loop that checks total numbers letters, number and symbols
#along with toal symbols and whitespaces
for i in Letters:
    total_letters=total_letters+Letters[i]
    if i in Symbols_and_Numbers:
        symbols=symbols+Letters[i]
white_space=Letters[' ']

#for loop to check the letters having probability gretater than 0.5 and putting them in a dictionary
#it breka if '@' becaus that seperetaes the letters and symbols
for i in Letters:
    if(i=='@'):
        break
    probablity=Letters[i]/float(total_letters)
    probablity=round(probablity,4)
    if(probablity>=0.05):
        probablity_of_each[i]=probablity
    else:
        remaining = remaining+probablity
probablity_of_each['symbols']=round(symbols/total_letters,4)
probablity_of_each['space']=round(white_space/total_letters,4)
probablity_of_each['remaining']=round(remaining,4)

#drawing the piechart

piechart=Turtle()
piechart.penup()
piechart.sety(-RADIUS)
piechart.pendown()
k=0
# for loop to draw each letters having probablity greater than 0.05
for i in probablity_of_each:
    piechart.fillcolor(COLORS[k])
    piechart.begin_fill()
    piechart.circle(RADIUS,probablity_of_each[i]*360)
    position=piechart.position()
    piechart.goto(0,0)
    piechart.end_fill()
    piechart.setposition(position)
    k=k+1
    if(k>=11):
        k=0

piechart.penup()
piechart.sety(-LABEL_RADIUS)
#for loop to write the labels for each letters with 0.05 or high probablity and symbols space and remainig
for i in probablity_of_each:
    piechart.circle(LABEL_RADIUS,(probablity_of_each[i]*360) /2)
    piechart.write(i,align="center",font=FONT)
 
    piechart.circle(LABEL_RADIUS,(probablity_of_each[i]*360) /2)

#for loop to show the probablity of each
piechart.penup()
piechart.sety(-LABEL_RADIUS*1.3)
for i in probablity_of_each:
    piechart.circle(LABEL_RADIUS*1.3,(probablity_of_each[i]*360) /2)
    piechart.write(probablity_of_each[i],align="center",font=FONT)
    piechart.circle(LABEL_RADIUS*1.3,(probablity_of_each[i]*360) /2)
piechart.goto(0,0)
piechart.hideturtle()
    



    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
