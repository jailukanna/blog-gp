---
title: turtle example pd saper create board (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'pd saper create board'

Functions in program: 
* `def create_board(rows, columns, num_of_bombs):`
* `def add_how_many_bombs_nearby(board, bombs_cor, direction):`
* `def plant_bombs(rows, columns, num_of_bombs, board): #this function plants bombs and returns their coordinates`
* `def single_field():`

## python pd saper create board

Python turtle example: pd saper create board

```python
from random import randint
from give_coordinates import give_coordinates

#----------------creating random board-------------------

def single_field():
    return [False, False, False, 0] #[if_visible, if_flagged, has_bomb, number_of_bombs_surrounding]

def plant_bombs(rows, columns, num_of_bombs, board): #this function plants bombs and returns their coordinates
    bombs_cor = []
    i = 0 #control variable for while loop
    while i != num_of_bombs:
        r = randint(0,rows-1) #which row_index would bomb be in
        c = randint(0, columns-1) #which column_index would bomb be in
        cor = [r,c]
        if cor not in bombs_cor:
            board[r][c][2] = True
            i += 1
            bombs_cor.append(cor)
    return bombs_cor

#directions: {"n":"north", "s":"south", "e":"east", "w":"west", "nw":"north-west", "sw":"south-west", "ne":"north-east", "se":"south-east"}
#directions are defined from the bomb perspective
def add_how_many_bombs_nearby(board, bombs_cor, direction):
    for i in range(len(bombs_cor)):
        if direction == "n" and bombs_cor[i][0] != 0: #asserting the bomb is not in the highest row
            board[bombs_cor[i][0]-1][bombs_cor[i][1]][3] += 1
        elif direction == "s" and bombs_cor[i][0] < len(board)-1: #asserting the bomb is not in the lowest row
            board[bombs_cor[i][0]+1][bombs_cor[i][1]][3] += 1
        elif direction == "w" and bombs_cor[i][1] != 0: #asserting the bomb is not in the boarder left column
            board[bombs_cor[i][0]][bombs_cor[i][1]-1][3] += 1
        elif direction == "e" and bombs_cor[i][1] < len(board)-1: #asserting the bomb is not in the boarder right column
            board[bombs_cor[i][0]][bombs_cor[i][1]+1][3] += 1
        elif direction == "ne" and bombs_cor[i][0] != 0 and bombs_cor[i][1] < len(board)-1:
            board[bombs_cor[i][0]-1][bombs_cor[i][1]+1][3] += 1
        elif direction == "nw" and bombs_cor[i][0] != 0 and bombs_cor[i][1] != 0:
            board[bombs_cor[i][0]-1][bombs_cor[i][1]-1][3] += 1
        elif direction == "sw" and bombs_cor[i][0] < len(board)-1 and bombs_cor[i][1] != 0:
            board[bombs_cor[i][0]+1][bombs_cor[i][1]-1][3] += 1
        elif direction == "se" and bombs_cor[i][0] < len(board)-1 and bombs_cor[i][1] < len(board)-1:
            board[bombs_cor[i][0]+1][bombs_cor[i][1]+1][3] += 1
    return board

def create_board(rows, columns, num_of_bombs):
    assert num_of_bombs <= rows*columns
    board = []
    for i in range(rows): #creating empty board (no bombs)
        row = []
        for j in range(columns):
            row.append(single_field())
        board.append(row)
    bombs_cor = plant_bombs(rows,columns, num_of_bombs, board)
    directions = ["n","s","w","e","ne","nw","se","sw"]
    for direction in directions:
        add_how_many_bombs_nearby(board,bombs_cor,direction)
    give_coordinates(board, 450, 450)
    return board



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
