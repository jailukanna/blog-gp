---
title: turtle example labyrinth (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'labyrinth'

Functions in program: 
* `def draw_tile2(Tile):`
* `def draw_tile(Tile):`
* `def draw_mark(mark_num):`
* `def draw_board(board, x=5, y=155):`
* `def init_board():`

Modules used in program: 
* `import turtle`
* `import random`

## python labyrinth

Python turtle example: labyrinth

```python
# Labrynth generator for The A-MAZE-ing Labyrinth board game

import random
import turtle
from turtle import fd, bk, rt, lt, up, down
from enum import Enum

Orientation = Enum('Orientation', 'North East South West')
Shape = Enum('shape', 'Straight Corner Tee')


class Tile(object):

    """Represents a single board tile."""

    def __init__(self, shape, orientation, mark):
        self.shape = shape
        self.orientation = orientation
        self.mark = mark


def init_board():
    """Create the board data structure.

    Returns
    -------
    list of list of Tile
        Seven rows of seven tiles, plus an "eighth row" with just the free tile"""

    pieces = []  # collection of free tiles

    for i in range(13):
        pieces.append(Tile(Shape.Straight, random.choice(list(Orientation)), None))

    for i in range(13, 22):
        pieces.append(Tile(Shape.Corner, random.choice(list(Orientation)), None))

    for i in range(22, 28):
        pieces.append(Tile(Shape.Corner, random.choice(list(Orientation)), i - 10))

    for i in range(28, 34):
        pieces.append(Tile(Shape.Tee, random.choice(list(Orientation)), i - 10))

    random.shuffle(pieces)  # Shuffle the free tiles

    # Frozen tiles are directly assigned to the board,
    # others are assigned randomly
    board = [
        # row 0
        [Tile(Shape.Corner, Orientation.North, None),
            pieces[0],
            Tile(Shape.Tee, Orientation.North, 0),
            pieces[1],
            Tile(Shape.Tee, Orientation.North, 1),
            pieces[2],
            Tile(Shape.Corner, Orientation.East, None)],

        # row 1
        [pieces[3],
            pieces[4],
            pieces[5],
            pieces[6],
            pieces[7],
            pieces[8],
            pieces[9]],

        # row 2
        [Tile(Shape.Tee, Orientation.West, 2),
            pieces[10],
            Tile(Shape.Tee, Orientation.West, 3),
            pieces[11],
            Tile(Shape.Tee, Orientation.North, 4),
            pieces[12],
            Tile(Shape.Tee, Orientation.East, 5)],

        # row 3
        [pieces[13],
            pieces[14],
            pieces[15],
            pieces[16],
            pieces[17],
            pieces[18],
            pieces[19]],

        # row 4
        [Tile(Shape.Tee, Orientation.West, 6),
            pieces[20],
            Tile(Shape.Tee, Orientation.South, 7),
            pieces[21],
            Tile(Shape.Tee, Orientation.East, 8),
            pieces[22],
            Tile(Shape.Tee, Orientation.East, 9)],

        # row 5
        [pieces[23],
            pieces[24],
            pieces[25],
            pieces[26],
            pieces[27],
            pieces[28],
            pieces[29]],

         # row 6
        [Tile(Shape.Corner, Orientation.West, None),
            pieces[30],
            Tile(Shape.Tee, Orientation.South, 10),
            pieces[31],
            Tile(Shape.Tee, Orientation.South, 11),
            pieces[32],
            Tile(Shape.Corner, Orientation.South, None)],

        # the free tile
        [pieces[33]]]

    return board


def draw_board(board, x=5, y=155):
    """Given a board, draws it using turtle."""

    turtle.setworldcoordinates(0, 0, 160, 160)
    turtle.speed(0)

    # Draw the grid
    up()
    turtle.pensize(1)
    turtle.color("gray")
    turtle.setpos(x, y)
    for i in range(8):
        down()
        fd(70)
        up()
        bk(70)
        rt(90)
        fd(10)
        lt(90)

    turtle.setpos(x, y)
    rt(90)
    for i in range(8):
        down()
        fd(70)
        up()
        bk(70)
        lt(90)
        fd(10)
        rt(90)

    lt(90)
    turtle.setpos(x, y)

    # Draw the tiles
    turtle.pensize(3)
    turtle.color("black")
    for i in range(7):
        for j in range(7):
            draw_tile(board[i][j])
            fd(10)

        bk(70)
        rt(90)
        fd(10)
        lt(90)

    turtle.setpos(x + 80, y)
    draw_tile(board[7][0])


def draw_mark(mark_num):
    """Draws a tile's mark number inside a blue square."""

    fd(4)
    rt(90)
    fd(6.5)
    lt(90)
    turtle.write(mark_num)
    bk(1)
    rt(90)
    bk(3.5)
    lt(90)

    # The square
    turtle.color("blue")
    down()
    for _ in range(4):
        fd(4)
        rt(90)
    up()
    turtle.color("black")

    bk(3)
    rt(90)
    bk(3)
    lt(90)


def draw_tile(Tile):
    """Orients to draw the Tile, calls draw_tile2, then unorients"""

    if Tile.orientation == Orientation.North:
        draw_tile2(Tile)

    elif Tile.orientation == Orientation.East:
        fd(10)
        rt(90)
        draw_tile2(Tile)
        lt(90)
        bk(10)

    elif Tile.orientation == Orientation.South:
        fd(10)
        rt(90)
        fd(10)
        rt(90)
        draw_tile2(Tile)
        lt(90)
        bk(10)
        lt(90)
        bk(10)

    elif Tile.orientation == Orientation.West:
        lt(90)
        bk(10)
        draw_tile2(Tile)
        fd(10)
        rt(90)

    else:
        raise Exception("Unknown orientation value {}".format(Tile.orientation))

    if Tile.mark is not None:
        draw_mark(Tile.mark)


def draw_tile2(Tile):
    """Draws the Tile's path shape."""

    if Tile.shape == Shape.Straight:
        rt(90)
        fd(2)
        lt(90)

        down()
        fd(10)
        up()

        rt(90)
        fd(6)
        rt(90)

        down()
        fd(10)
        up()

        rt(90)
        fd(8)
        rt(90)

    elif Tile.shape == Shape.Corner:
        fd(2)
        rt(90)
        fd(2)

        down()
        fd(8)
        up()

        lt(90)
        fd(6)
        lt(90)

        down()
        fd(2)
        rt(90)
        fd(2)
        up()

        lt(90)
        fd(6)
        lt(90)

        down()
        fd(8)
        up()

        fd(2)
        rt(90)
        fd(2)
        rt(90)

    elif Tile.shape == Shape.Tee:
        rt(90)
        fd(8)
        lt(90)

        down()
        fd(2)
        rt(90)
        fd(2)
        lt(90)
        up()

        fd(6)
        lt(90)

        down()
        fd(2)
        rt(90)
        fd(2)
        lt(90)
        up()

        fd(6)
        lt(90)

        down()
        fd(10)
        up()

        rt(90)
        fd(2)
        rt(90)

gboard = init_board()
draw_board(gboard)

turtle.getscreen()._root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
