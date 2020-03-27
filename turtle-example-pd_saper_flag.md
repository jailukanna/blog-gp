---
title: turtle example pd saper flag (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'pd saper flag'

Functions in program: 
* `def flag_field(board,x,y):`
* `def update_flagged(board):`

Modules used in program: 
* `import turtle`

## python pd saper flag

Python turtle example: pd saper flag

```python
from mouse import ustaw_guziki_myszy, daj_zdarzenie, ini_myszki
from initate_graphics import rectangle
window_width = 450
window_hight = 450
import turtle

def update_flagged(board):
    count = 0
    for i in range(len(board)):
        for j in range(len(board[0])):
            if board[i][j][1]:
                count += 1
    return count

def flag_field(board,x,y):
    x_cor = turtle.xcor()
    y_cor = turtle.ycor()
    for i in range(len(board)):
        for j in range(len(board[0])):
            if board[i][j][1] and board[i][j][4] <= x < board[i][j][4]+window_width/len(board[0])\
                    and board[i][j][5] >= y > board[i][j][5]+window_hight/len(board):
                turtle.pencolor("Dark Slate Blue")
                turtle.fillcolor("Lavender")
                turtle.penup()
                turtle.goto(board[i][j][4],board[i][j][5])
                turtle.pendown()
                turtle.begin_fill()
                rectangle(window_hight/len(board),window_width/len(board[0]))
                turtle.end_fill()
            elif (not board[i][j][1]) and board[i][j][4] <= x < board[i][j][4]+window_width/len(board[0])\
                    and board[i][j][5] >= y > board[i][j][5]+window_hight/len(board):
                turtle.penup()
                turtle.goto(board[i][j][4]+(window_width/(len(board[0]))*(5/8)),board[i][j][5]+(window_hight/(2*len(board))))
                turtle.fillcolor("Tomato")
                turtle.circle((window_width/len(board[0]))*(1/8))
    turtle.goto(x_cor,y_cor)




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
