---
title: turtle example draw olympic logo (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'draw olympic logo'

Functions in program: 
* `def create_circle( turtle_object, coordinates, color, radius ):`

## python draw olympic logo

Python turtle example: draw olympic logo

```python
from turtle import Turtle

def create_circle( turtle_object, coordinates, color, radius ):
    """Creates a circle with **coordinates** as start point."""
    # Pull up the pen first.
    turtle_object.penup()
    # Go to initial coordinates.
    turtle_object.goto(coordinates[0], coordinates[1]);
    # Pendown to start drawing.
    turtle_object.pendown();
    # Use this color.
    turtle_object.color(color)
    # with this radius.
    turtle_object.circle(radius);



##################### MAIN #####################

# Create new **Turtle** object.
# Fans of harry potter movies can go crazy now
myrtle = Turtle()
# Set width for fat lines.
myrtle.width(25)
# Olympic symbol start point.
start_x = -200
start_y = 0
# Olympic ring colors.
colors = ["blue","yellow","black","green","red"]
# Circle radius.
RADIUS = 60

# Draw now.
for color in colors:
    create_circle(myrtle, [start_x, start_y], color, RADIUS)

    if (start_y == 0):
        start_y = -50
    else:
        start_y = 0

    start_x += 90

# Hide turtle / cursor.
myrtle.hideturtle();
# keep window open
myrtle.getscreen()._root.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
