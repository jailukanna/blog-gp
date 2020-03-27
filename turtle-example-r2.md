---
title: turtle example r2 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'r2'

Functions in program: 
* `def find(n, print_log=False, simulate=False):`

## python r2

Python turtle example: r2

```python
"""
Riddle :
Room with 100 drawers each drawer contains a unique real number between 0..99
First man gets into the room and can open all drawers but can switch drawers only once
Second man will enter afterwards and will be asked to find a random number in 50 attempts
Both men can talk only before first man will get in and decide on strategy
Whats the best strategy ?
"""
from random import shuffle
from turtle import Turtle, Screen

NUM_DRAWERS = 100
NUM_ATTEMPTS_ALLOWED = int(NUM_DRAWERS / 2)
NUM_TO_FIND = 1
SIMULATE_FIRST = True


class VisualSimulation:
    FONT = ("Arial", 12, "normal")
    MARKER_SHAPE = "circle"
    WINDOW_WIDTH, WINDOW_HEIGHT = 1800, 300
    MARKER_SPEED = 10  # fastest-no-animation: 0, fast: 10, normal: 6, slow: 3, slowest: 1
    TEXT_ALIGNMENT = "center"

    def __init__(self, drawers_keys, drawers_values):
        super().__init__()
        self.drawers_keys = drawers_keys
        self.drawers_values = drawers_values
        self.t = Turtle(shape=self.MARKER_SHAPE)
        self.canvas = Screen()

    def write_step(self, n):
        self.t.penup()
        y = self.t.ycor()
        self.t.sety(-7)
        self.t.write(str(n) + '#' + str(self.drawers_keys[n]), align=self.TEXT_ALIGNMENT, font=self.FONT)
        self.t.sety(y)
        self.t.pendown()

    def simulate(self):
        self.setup()
        self.draw_x_axis()

        # start moving between drawers
        for idx, step in enumerate(self.drawers_keys):
            if step != self.drawers_keys[-1]:
                next_move = self.drawers_keys[idx + 1]
                steps = 30  # render quality, in some way..
                if next_move > step:
                    distance = next_move - step
                    self.t.right(90)  # turn 90 degrees
                    self.t.circle(distance / 2, -180, steps=steps)
                else:
                    distance = step - next_move
                    self.t.left(90)
                    self.t.circle(distance / 2, 180, steps=steps)
                self.write_step(idx + 1)
            self.t.setheading(0)

        self.canvas.exitonclick()

    def draw_x_axis(self):
        self.t.speed(0)
        for n in range(100):
            self.t.goto(n, -9)
            if n % 5 == 0:
                self.t.write(n, align=self.TEXT_ALIGNMENT, font=self.FONT)
        self.t.penup()
        self.t.hideturtle()
        self.t.goto(self.drawers_keys[0], -5)
        self.t.showturtle()
        self.t.pendown()
        self.t.speed(self.MARKER_SPEED)

    def setup(self):
        num_to_find = self.drawers_values[self.drawers_keys[-1]]
        self.canvas.title('opened [ %d ] drawers to find number [ %d ]' % (len(self.drawers_keys), num_to_find))
        self.canvas.setup(self.WINDOW_WIDTH, self.WINDOW_HEIGHT)
        self.canvas.setworldcoordinates(0, -10, 100, 10)

drawers = []
drawers.extend(range(0, NUM_DRAWERS))  # fill drawers with numbers
print('before shuffle\n', drawers)
shuffle(drawers)
print('after shuffle\n', drawers)


def find(n, print_log=False, simulate=False):
    n_found = False
    current_drawer = n
    opened_drawers = 0
    movement = []
    while not n_found:
        opened_drawers += 1
        movement.append(current_drawer)
        if print_log:
            print('open drawer %d see number %d' % (current_drawer, drawers[current_drawer]))
        if drawers[current_drawer] == n:
            n_found = True
        else:
            current_drawer = drawers[current_drawer]
    if simulate:
        VisualSimulation(movement, drawers).simulate()
        print('simulating..')
    return opened_drawers

print('trying to find number %d' % NUM_TO_FIND)
attempts = find(NUM_TO_FIND, True, SIMULATE_FIRST)
print('number attempts: %d' % attempts)
if attempts > NUM_ATTEMPTS_ALLOWED:
    print('need to switch, finding which drawers to switch')
    middle_drawer, last_drawer = 0, 0
    found = False
    cur_drawer = NUM_TO_FIND
    num_attempts = 0
    while not found:
        num_attempts += 1
        if num_attempts == NUM_DRAWERS / 2:
            middle_drawer = cur_drawer
        if drawers[cur_drawer] is NUM_TO_FIND:
            last_drawer = cur_drawer
            found = True
        else:
            cur_drawer = drawers[cur_drawer]
    print('switching drawer [%d] with drawer [%d]' % (last_drawer, middle_drawer))
    temp = drawers[last_drawer]
    drawers[last_drawer] = drawers[middle_drawer]
    drawers[middle_drawer] = temp
    print('after switch', drawers)
    print('showing how many attempts to find each number')
    for i in range(NUM_DRAWERS):
        num_attempts = find(i)
        print('found number %d in %d attempts' % (i, num_attempts))
        assert num_attempts <= NUM_ATTEMPTS_ALLOWED
else:
    print('run again to shuffle drawers')

    
    
'''

2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10| 1 
--------------------------------------
1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10
מקרה קיצון נח לדוגמא שהמספרים בסדר עולה מתחילים ב2 ובמגירה העשירית נמצא המספר 1
שורה תחתונה זה המגירה
כדי למצוא את מספר "1" : 
פותח מגירה 1 רואה את המספר 2
פותח מגירה 2 רואה את המספר 3
פותח מגירה 3 רואה את המספר 4
פותח מגירה 4 רואה את המספר 5
פותח מגירה 5 רואה את המספר 6
פותח מגירה 6 רואה את המספר 7
פותח מגירה 7 רואה את המספר 8 
פותח מגירה 8 רואה את המספר 9
פותח מגירה 9 רואה את המספר 10
פותח מגירה 10 רואה את המספר 1
סה"כ מגירות נפתחו: 9
ההחלפה : מחליפים את המגירה האמצעית (מספר ההחלפות חלקי 2) עם המגירה האחרונה

a
2 | 3 | 4 | 5 | 1 | 7 | 8 | 9 | 10| 6 
--------------------------------------
1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10

עכשיו כל מספר שנרצה למצוא ימצא תוך 5 ניסיונות





'''

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
