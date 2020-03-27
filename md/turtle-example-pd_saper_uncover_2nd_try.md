---
title: turtle example pd saper uncover 2nd try (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'pd saper uncover 2nd try'

Functions in program: 
* `def uncover_field(board,x,y):`
* `def uncover_nearby(i, j, board,empties):`
* `def collect_empty(i, j, board,empties):`

Modules used in program: 
* `import turtle`

## python pd saper uncover 2nd try

Python turtle example: pd saper uncover 2nd try

```python
import turtle
from create_board import create_board
window_width = 450
window_hight = 450

def collect_empty(i, j, board,empties):
    if board[i][j][3] == 0 and (not board[i][j][0]):
        empties.append([i,j])

def uncover_nearby(i, j, board,empties):
    if i != 0:
        collect_empty(i-1,j,board,empties)
        board[i-1][j][0] = True
    if i != len(board)-1:
        collect_empty(i+1,j,board,empties)
        board[i+1][j][0] =True
    if j != 0:
        collect_empty(i,j-1,board,empties)
        board[i][j-1][0] = True
    if j != len(board[0])-1:
        collect_empty(i,j+1,board,empties)
        board[i][j+1][0] = True
    if i != 0 and j != 0:
        collect_empty(i-1,j-1,board,empties)
        board[i-1][j-1][0] = True
    if i != 0 and j != len(board[0])-1:
        collect_empty(i-1,j+1,board,empties)
        board[i-1][j+1][0] = True
    if i != len(board)-1 and j != 0:
        collect_empty(i+1,j-1,board,empties)
        board[i+1][j-1][0] = True
    if i != len(board)-1 and j != len(board[0])-1:
        collect_empty(i+1,j+1,board,empties)
        board[i+1][j+1][0] = True


def uncover_field(board,x,y):
    for i in range(len(board)):
        for j in range(len(board[0])):
            if board[i][j][4] <= x < board[i][j][4]+window_width/len(board[0])\
                    and board[i][j][5] <= y < board[i][j][5]+window_hight/len(board):
                board[i][j][0] = True
                if board[i][j][3] == 0:
                    empties = []
                    empties.append([i,j])
                    while len(empties) != 0:
                        i, j = empties[0]
                        uncover_nearby(i,j,board,empties)
                        del empties[0]

board = create_board(5,5,4)
for line in board:
    print(line)
x = int(input("x"))
y = int(input("y"))
uncover_field(board,x,y)
for i in range(len(board)):
    print(board[i])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
