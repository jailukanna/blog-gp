---
title: tkinter example graphturtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'graphturtle'

Functions in program: 
* `def sh():`
* `def showcase():`
* `def draw_graph(graph):`
* `def arbitrary(limit, interval, expression):`
* `def x_cubed(limit, interval):`
* `def tanh_x(limit, interval):`
* `def cosh_x(limit, interval):`
* `def sinh_x(limit, interval):`
* `def tan_x(limit, interval):`
* `def cos_x(limit, interval):`
* `def sin_x(limit, interval):`
* `def power_graph(n, limit, interval):`

Modules used in program: 
* `import tkinter.simpledialog`
* `import tkinter.messagebox`
* `import sys`
* `import _tkinter`
* `import traceback`
* `import math`
* `import turtle`

## python graphturtle

Python tkinter example: graphturtle

```python
"""Code I wrote in school to graph stuff"""

import turtle
import math
import traceback
import _tkinter
import sys
import tkinter.messagebox
import tkinter.simpledialog


def power_graph(n, limit, interval):
    curr_x = -limit
    values = {}

    while curr_x < limit:
        values[curr_x] = curr_x ** n

        curr_x += interval

    return values


def sin_x(limit, interval):
    curr_x = -limit
    values = {}

    while curr_x < limit:
        values[curr_x] = math.sin(curr_x)

        curr_x += interval

    return values


def cos_x(limit, interval):
    curr_x = -limit
    values = {}

    while curr_x < limit:
        values[curr_x] = math.cos(curr_x)

        curr_x += interval

    return values


def tan_x(limit, interval):
    curr_x = -limit
    values = {}

    while curr_x < limit:
        values[curr_x] = math.tan(curr_x)

        curr_x += interval

    return values


def sinh_x(limit, interval):
    curr_x = -limit
    values = {}

    while curr_x < limit:
        values[curr_x] = math.sinh(curr_x)

        curr_x += interval

    return values


def cosh_x(limit, interval):
    curr_x = -limit
    values = {}

    while curr_x < limit:
        values[curr_x] = math.cosh(curr_x)

        curr_x += interval

    return values


def tanh_x(limit, interval):
    curr_x = -limit
    values = {}

    while curr_x < limit:
        values[curr_x] = math.tanh(curr_x)

        curr_x += interval

    return values


def x_cubed(limit, interval):
    curr_x = -limit
    values = {}

    while curr_x < limit:
        values[curr_x] = curr_x ** 3

        curr_x += interval


def arbitrary(limit, interval, expression):
    curr_x = -limit
    values = {}

    while curr_x < limit:
        print(curr_x)
        print(curr_x ** math.pi)
        values[curr_x] = expression(curr_x)

        curr_x += interval

    return values


def draw_graph(graph):

    scale = 10

    terrapin.hideturtle()

    isfirst = True

    for x, val in zip(graph.keys(), graph.values()):

        if isfirst:
            terrapin.penup()
            terrapin.setpos(x * scale, val * scale)
            terrapin.pendown()
            isfirst = False

        terrapin.setpos(x * scale, val * scale)


def showcase():

    print("Sin")
    draw_graph(sin_x(cutoff, precision))
    terrapin.color("red")

    print("Hyp sin")
    draw_graph(sinh_x(cutoff, precision))
    terrapin.color("green")

    print("Cos")
    draw_graph(cos_x(cutoff, precision))
    terrapin.color("blue")

    print("Hyp cos")
    draw_graph(cosh_x(cutoff, precision))
    terrapin.color("pink")

    print("Tan")
    draw_graph(tan_x(cutoff, precision))
    terrapin.color("purple")

    print("Hyp tan")
    draw_graph(tanh_x(cutoff, precision))
    terrapin.color("orange")

    terrapin.color("black")


# noinspection PyBroadException
def sh():

    info = """
Expressions must be valid python snippets with x as the variable or varied
E.G math.sqrt(x)
By default, math is imported
To import libraries, you can use __import__()
This allows unsandboxed behavior, so be careful!
"""

    tkinter.messagebox.showinfo("Info", info)

    while True:

        raw_exp = tkinter.simpledialog.askstring("Expression", "Type in an expression to graph")

        if raw_exp in ("test", "showcase"):
            showcase()
            continue

        elif raw_exp is None:
            sys.exit(0)

        try:
            exp = eval("lambda x: " + raw_exp)
            exp(1)

        except Exception as error:

            print("Error with expression: ")
            print(traceback.format_exc())

            tkinter.messagebox.showerror("Error", error.args[0].capitalize())

            continue

        print("Graphing `{0}` from -{1} to {1}".format(raw_exp, cutoff))
        graph_out = arbitrary(cutoff, precision, exp)

        print("Plotting...")
        draw_graph(graph_out)

        print("Plotting complete")

screen = turtle.Screen()
terrapin = turtle.Turtle()
terrapin.speed(0)

cutoff = 30
precision = 0.1

try:
    sh()

except _tkinter.TclError:
    sys.exit(0)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
