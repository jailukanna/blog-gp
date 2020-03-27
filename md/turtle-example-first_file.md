---
title: turtle example first file (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'first file'


## python first file

Python turtle example: first file

```python
#this program demonstrates some python graphic abilities
# the following URL gives a short tutorial on using the turtle
# http://www.eg.bucknell.edu/~hyde/Python3/TurtleDirections.html

# turtle is used to draw on screen
from turtle import * # star is a wildcard

# set colors: red line, blue fill
color('green', 'blue')

begin_fill() # fill in the shape we draw

while True:
    speed(0)
    forward(275) # go forward 200 pixels
    left(245) #go forward 170 degrees
    back(100)
    right(30)

    # repeat until we return to starting position
    if abs(pos()) < 1:         
        break 
end_fill() # fill in shape 
done() # tell python that turtle is done drawing 
# the following is a loop 
# repeat until we return to starting position 
# while abs(pos()) >= 1:
    # forward(350)
    # left(145

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
