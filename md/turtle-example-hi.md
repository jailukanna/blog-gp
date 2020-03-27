---
title: turtle example hi (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'hi'


Modules used in program: 
* `import turtle_font`

## python hi

Python turtle example: hi

```python
# hi.py
# An example of how to use your turtle_font module

from turtle import *
import turtle_font

# change to an actual turtle
shape("turtle")

# change line width
pensize(5)

# Move to start
penup()
setup(width=600, height=300)
setposition(-250,0)
pendown()

# Write Hi
turtle_font.upper_h()
turtle_font.lower_i()

# Hide the turtle after completing the message
hideturtle()

# Tell tkinter to stop waiting for turtle instructions
done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
