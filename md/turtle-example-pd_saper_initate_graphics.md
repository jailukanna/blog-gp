---
title: turtle example pd saper initate graphics (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'pd saper initate graphics'

Functions in program: 
* `def ini_graphics(board):`
* `def write_info(info, hight, width):`
* `def draw_board(board, field_hight, field_width):`
* `def rectangle(a,b):`

Modules used in program: 
* `import turtle`

## python pd saper initate graphics

Python turtle example: pd saper initate graphics

```python
import turtle
from create_board import create_board
#-----------------drawing parameters-------------------------
window_width = 450
window_hight = 450
#------------------------------------------------------------


#every painted element will start and end from the left bottom corner, seth 90
#every written element begins set at the centre of the writing

def rectangle(a,b):
    for i in range(2):
        turtle.fd(a)
        turtle.right(90)
        turtle.fd(b)
        turtle.right(90)

def draw_board(board, field_hight, field_width):
    turtle.pencolor("Dark Slate Blue")
    turtle.fillcolor("Lavender")
    turtle.pendown()
    turtle.begin_fill()
    rectangle(window_hight,window_width)
    turtle.end_fill()
    for i in range(len(board)):
        print(len(board))
        for j in range(len(board[0])):
            print(len(board[0]))
            rectangle(field_hight,field_width)
            turtle.right(90)
            turtle.fd(field_width)
            turtle.left(90)
        turtle.left(90)
        turtle.fd(window_width)
        turtle.right(90)
        turtle.fd(field_hight)
    turtle.left(180)
    turtle.fd(window_hight)
    turtle.left(180)

def write_info(info, hight, width):
    turtle.pencolor("Dark Slate Blue")
    turtle.fillcolor("Misty Rose")
    turtle.begin_fill()
    rectangle(hight, width)
    turtle.end_fill()
    turtle.right(90)
    turtle.fd(width/2)
    turtle.left(90)
    turtle.penup()
    turtle.fd(hight/3)
    turtle.write(info, align="center", font=("Arial", 10, "normal"))
    turtle.backward(hight/3)
    turtle.pendown()
    turtle.left(90)
    turtle.fd(width/2)
    turtle.right(90)



def ini_graphics(board):
    turtle.tracer(0,0)
    turtle.seth(90)
    turtle.penup()
    turtle.goto(-window_width/2,-window_width/2)
    field_width = window_width/len(board[0])
    field_hight = window_hight/len(board)
    draw_board(board,field_hight,field_width)
    turtle.fd(window_hight)
    write_info("hello_world", window_hight/10, window_width)
    turtle.update()
    turtle.exitonclick()




ini_graphics(create_board(5,5,6))







```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
