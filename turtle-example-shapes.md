---
title: turtle example shapes (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'shapes'

Functions in program: 
* `def teach(inst, *classes, **attribs):`
* `def make_turtle(*classes, **attribs):`

Modules used in program: 
* `import random`
* `import math`

## python shapes

Python turtle example: shapes

```python
#!/usr/bin/env python
# encoding: utf-8
from __future__ import division

from turtle import Turtle
import math
import random


class ShapefulTurtle(Turtle):
    """Teach the turtle some shapes."""

    def rectangle(self, width, height):
        """Draw a rectangle.

                +-------+
                |       |
         height |       |
                |â¾      |
                >-------+
                  width

        """
        for dummy in 'bottom right', 'top left':
            self.forward(width)
            self.left(90)
            self.forward(height)
            self.left(90)

        # This is a more concise formulation:
        #for length in width, height, width, height:
        #    self.forward(length)
        #    self.left(90)

    def square(self, size):
        """Draw a square."""
        self.rectangle(size, size)

    def triangle(self, a, b, c):
        r"""Draw a triangle by the length of its sides.

            +
           /Î±\
        c /   \ b
         /Î²   Î³\
        >-------+
            a

        """
        assert a + b >= c
        assert a + c >= b
        assert b + c >= a
        #assert all(a + b + c - side >= side for side in (a, b, c))

        alpha = math.acos((b**2 + c**2 - a**2) / (2 * b * c))
        beta  = math.acos((a**2 + c**2 - b**2) / (2 * a * c))
        gamma = math.acos((a**2 + b**2 - c**2) / (2 * a * b))
        # We could find one of the angles through the measure of the other two.

        self.forward(a)
        self.left(180 - math.degrees(gamma))
        self.forward(b)
        self.left(180 -  math.degrees(alpha))
        self.forward(c)
        self.left(180 - math.degrees(beta))

        # More concise:
        #for length, angle in ((a, gamma), (b, alpha), (c, beta)):
        #    self.left(180 - math.degrees(angle))
        #    self.forward(length)

    def trapezoid(self, bottom, angle, height):
        r"""Draw an isosceles trapezoid.

              top
            +------+
           /.       \
          / . height \ slope
         /  .    angle\
        >--------------+
             bottom

        """
        assert 0 < angle < 180

        slope = height / math.sin(math.radians(angle))
        top = bottom - 2 * (height / math.tan(math.radians(angle)))

        self.forward(bottom)
        self.left(180 - angle)
        self.forward(slope)
        self.left(angle)
        self.forward(top)
        self.left(angle)
        self.forward(slope)
        self.left(angle - 180)

        # More concise:
        #for length, turn in ((bottom, 180 - angle),
        #                      (slope, angle),
        #                      (top, angle),
        #                      (slope, angle - 180)):
        #    self.forward(length)
        #    self.left(turn)

    def poly(self, shape, scale=1):
        """Draw a `turtle.Shape` polygon.

        All available shapes can be found through `turtle.getshapes()`.

        """
        if isinstance(shape, basestring):
            shape = self.getscreen()._shapes[shape]

        xoff, yoff = self.pos()
        for x, y in shape._data:
            self.goto(xoff + x * scale, yoff + y * scale)


class DashedTurtle(Turtle):
    """A turtle which moves in dashed lines."""

    length = 10

    def toggle(self):
        if self.isdown():
            self.up()
        else:
            self.down()

    def forward(self, distance):
        for dummy in xrange(self.length, distance + 1, self.length):
            super(DashedTurtle, self).forward(self.length)
            self.toggle()
        super(DashedTurtle, self).forward(distance % self.length)


class ShadowedTurtle(Turtle):
    """A turtle with line shadows on every forward stroke."""

    shadow_offset = 4, 3
    shadow_color = 'darkgray'

    def forward(self, distance):
        """For every visible forward motion, draw an offset shadow.

        This is flawed.
        """
        if self.isdown():
            # Normal forward motion
            super(ShadowedTurtle, self).forward(distance)
            end = self.pos()

            # Back to the starting point
            self.up()
            orig_tracer = self.tracer()
            self.tracer(False)
            self.backward(distance)

            # Offset movement
            xoff, yoff = self.shadow_offset
            self.goto(self.xcor() + xoff, self.ycor() + yoff)

            # Shadow drawing
            orig_color = self.color()
            self.color(self.shadow_color)
            self.down()
            super(ShadowedTurtle, self).forward(distance)

            # Final position
            self.up()
            self.goto(end)

            # Reset turtle state
            self.color(*orig_color)
            self.tracer(orig_tracer)
            self.down()


class PulsatingTurtle(Turtle):
    """A turtle fluctuating its width while moving."""

    max = 15

    def forward(self, distance):
        if not self.isdown():
            # Skip all the fun when we're not drawing anyways.
            super(PulsatingTurtle, self).forward(distance)
            return

        for i in xrange(distance):
            # The highlighted slice of the width function shows how it maps
            # to an inverted, offset (in both axes) abs() function.
            #
            #   width      I-------I
            #     ^
            # max |    x       x
            #     .   x x     x x     .
            #     .  x   x   x   x   .
            #     . x     x x     x .
            #   1 |x       x       x
            #     +----|---|---|--------> distance
            #      0 max-1 | 3(max-1)
            #          2(max-1)    ...
            new_width = self.max - abs((i % ((self.max - 1) * 2))
                                        - (self.max - 1))
            self.width(new_width)
            super(PulsatingTurtle, self).forward(1)


class SneakyTurtle(Turtle):
    """A turtle which is invisible during movement."""

    def forward(self, distance):
        self.hideturtle()
        super(SneakyTurtle, self).forward(distance)
        self.showturtle()


class InvertedTurtle(Turtle):
    """A confused turtle."""

    def forward(self, distance):
        super(InvertedTurtle, self).backward(distance)
    def backward(self, distance):
        super(InvertedTurtle, self).forward(distance)
    def left(self, angle):
        super(InvertedTurtle, self).right(angle)
    def right(self, angle):
        super(InvertedTurtle, self).left(angle)


class ShapeshiftingTurtle(Turtle):
    """A turtle which changes its shape before every motion."""

    def shapeshift(self):
        self.shape(random.choice(self.getscreen().getshapes()))

    def forward(self, distance):
        super(ShapeshiftingTurtle, self).forward(distance)
        self.shapeshift()

    def backward(self, distance):
        super(ShapeshiftingTurtle, self).backward(distance)
        self.shapeshift()

'''
These aliases will not be overridden.  Meh.

    fd = forward
    bk = back
    backward = back
    rt = right
    lt = left
    position = pos
    setpos = goto
    setposition = goto
    seth = setheading
    width = pensize
    up = penup
    pu = penup
    pd = pendown
    down = pendown
    st = showturtle
    ht = hideturtle
'''

### Demo time!

turtle = ShapefulTurtle()
turtle.triangle(100, 70, 90)
turtle.right(90)
turtle.trapezoid(150, 20, 15)
turtle.poly('turtle', 3)

class MyTurtle(SneakyTurtle, DashedTurtle, PulsatingTurtle, ShapefulTurtle):
    pass
# If SneakyTurtle is mixed in *after* PulsatingTurtle, the cursor will blink!

turtle = MyTurtle()
turtle.speed("fast")
turtle.rectangle(40, 50)


def make_turtle(*classes, **attribs):
    """Create a turtle from mixins."""

    return type("MyTurtle", classes + (ShapefulTurtle,), attribs)()

turtle = make_turtle(
        InvertedTurtle,
        ShapeshiftingTurtle,
        DashedTurtle,
        length=2,
    )
turtle.speed("slowest")
turtle.rectangle(100, 50)

turtle = PulsatingTurtle()
turtle.speed("fast")
turtle.right(45)
turtle.forward(80)

turtle = make_turtle(
        ShadowedTurtle,
    )
turtle.up()
turtle.goto(-200, -100)
turtle.width(3)
turtle.down()
turtle.rectangle(50, 100)


def teach(inst, *classes, **attribs):
    """Add behaviour to an instance.

    Useful for:

        >>> import turtle
        >>> teach(turtle._getpen(), ShapefulTurtle)

    """
    inst.__class__ = type('', classes + (inst.__class__,), attribs)

teach(turtle, SneakyTurtle)
turtle.speed("slowest")
turtle.left(30)
turtle.forward(200)

turtle.getscreen().exitonclick()
 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
