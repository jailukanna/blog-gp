---
title: turtle example Pong (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Pong'

Functions in program: 
* `def paddle_b_movement_down():`
* `def paddle_b_movement_up():`
* `def paddle_a_movement_down():`
* `def paddle_a_movement_up():`
* `def update_score():`
* `def start_game():`

## python Pong

Python turtle example: Pong

```python
from turtle import Turtle, Screen, Pen
s = Screen()
s.setup(800, 600)
s.screensize(780, 580)
s.bgcolor('black')
s.title("Pong Game by Perfect Web Solutions")
s.tracer(0)
# ball
ball = Turtle('circle', visible=False)
ball.color('white')
ball.up()
ball.dx = 0
ball.dy = 0
# Paddle A
paddle_a = Turtle('square', visible=False)
paddle_a.color('white')
paddle_a.up()
# Paddle B
paddle_b = Turtle('square', visible=False)
paddle_b.color('white')
paddle_b.up()
# Pen
pen = Turtle('circle', visible=False)
pen.color('white')
pen.write("Press SpaceBar to Start The Game!",
          align='center', font=("Arial", 22, 'bold'))
pen.up()
# Scores
score_a = 0
score_b = 0


def start_game():
    # Pen
    pen.clear()
    pen.goto(0, 250)
    pen.write("Player A: 0 | Player B: 0",
              align='center', font=("Arial", 22, 'bold'))
    # Paddle A Setup
    paddle_a.goto(-375, 0)
    paddle_a.shapesize(4, 1)
    paddle_a.st()
    # Paddle B Setup
    paddle_b.goto(375, 0)
    paddle_b.shapesize(4, 1)
    paddle_b.st()
    # Ball Setup
    ball.st()
    ball.dx = -2
    ball.dy = 2


def update_score():
    pen.clear()
    pen.write(f"Player A: {score_a} | Player B: {score_b}",
              align='center', font=("Arial", 22, 'bold'))


def paddle_a_movement_up():
    paddle_a.sety(paddle_a.ycor() + 10)


def paddle_a_movement_down():
    paddle_a.sety(paddle_a.ycor() - 10)


def paddle_b_movement_up():
    paddle_b.sety(paddle_b.ycor() + 10)


def paddle_b_movement_down():
    paddle_b.sety(paddle_b.ycor() - 10)


s.listen()
s.onkeypress(start_game, 'space')
# Paddle A Movement
s.onkeypress(paddle_a_movement_up, 'w')
s.onkeypress(paddle_a_movement_up, 'W')
s.onkeypress(paddle_a_movement_down, 's')
s.onkeypress(paddle_a_movement_down, 'S')
# Paddle B Movement
s.onkeypress(paddle_b_movement_up, 'Up')
s.onkeypress(paddle_b_movement_down, 'Down')

while True:

    ball.setx(ball.xcor() + ball.dx)
    ball.sety(ball.ycor() + ball.dy)
    if ball.ycor() > 290:
        ball.sety(290)
        ball.dy *= -1
    if ball.ycor() < -290:
        ball.sety(-290)
        ball.dy *= -1
    if ball.xcor() > 390:
        ball.goto(0, 0)
        ball.dx *= -1
        ball.dy *= 1
        score_a += 1
        update_score()
    if ball.xcor() < -390:
        ball.goto(0, 0)
        ball.dx *= -1
        ball.dy *= -1
        score_b += 1
        update_score()
    # Paddle and Ball Collisions
    if (ball.xcor() > 360 and ball.xcor() < 370) and (ball.ycor() < paddle_b.ycor() + 20 and ball.ycor() > paddle_b.ycor() - 20):
        ball.setx(360)
        ball.dx *= -1
    if (ball.xcor() < -360 and ball.xcor() > -370) and (ball.ycor() < paddle_a.ycor() + 20 and ball.ycor() > paddle_a.ycor() - 20):
        ball.setx(-360)
        ball.dx *= -1
    s.update()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
