---
title: turtle example ebay (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'ebay'

Functions in program: 
* `def paste_up(dummy_parameter):`
* `def release_drawing_canvas():`
* `def create_drawing_canvas(mark_centre_points = True):`

## python ebay

Python turtle example: ebay

```python
#-----Preamble-------------------------------------------------------#
#
# This section imports necessary functions and defines constant
# values used for creating the drawing canvas.  You should not change
# any of the code in this section.
#
 
# Import the functions needed to complete this assignment.  You
# should not need to use any other modules for your solution.
 
from turtle import *
from math import *
 
# Define constant values used in the main program that sets up
# the drawing canvas.  Do not change any of these values.
 
sheet_width = 200 # pixels
sheet_height = 500 # pixels
backing_margin = 20 # pixels
backing_width = sheet_width * 4 + backing_margin * 2
backing_height = sheet_height + backing_margin * 2
canvas_top_and_bottom_border = 150 # pixels
canvas_left_and_right_border = 300 # pixels
canvas_width = (backing_width + canvas_left_and_right_border)
canvas_height = (backing_height + canvas_top_and_bottom_border)
 
#
#--------------------------------------------------------------------#
 
 
 
#-----Functions for Creating the Drawing Canvas----------------------#
#
# The functions in this section are called by the main program to
# set up the drawing canvas for your image.  You should not change
# any of the code in this section.
#
 
# Set up the canvas and draw the background for the overall image
def create_drawing_canvas(mark_centre_points = True):
 
    # Set up the drawing canvas
    setup(canvas_width, canvas_height)
    # Draw as fast as possible
    tracer(False)
    # Colour the sky blue
    bgcolor('sky blue')
 
    # Draw the ground as a big green rectangle (sticking out of the
    # bottom edge of the drawing canvas slightly)
    overlap = 5 # pixels
    grass_height = 100 # pixels
    penup()
    goto(-(canvas_width // 2 + overlap),
         -(canvas_height // 2 + overlap)) # start at the bottom-left
    fillcolor('pale green')
    begin_fill()
    setheading(90) # face north
    forward(grass_height + overlap)
    right(90) # face east
    forward(canvas_width + overlap * 2)
    right(90) # face south
    forward(grass_height + overlap)
    end_fill()
 
    # Draw a nice warm sun peeking into the image
    penup()
    goto(-canvas_width // 2, canvas_height // 2)
    color('yellow')
    dot(350)
 
    # Draw a big fluffy white cloud in the sky
    goto(canvas_width // 3, canvas_height // 3)
    color('white')
    dot(200)
    setheading(200)
    forward(100)
    dot(180)
    setheading(0)
    forward(200)
    dot(160)
 
    # Draw the billboard's wooden backing as four frames
    # and some highlighted coordinates
    #
    # Outer rectangle
    goto(- backing_width // 2, - backing_height // 2) # bottom left
    pencolor('sienna'); fillcolor('tan'); width(3)
    begin_fill()
    pendown()
    setheading(90) # face north
    forward(backing_height)
    right(90) # face east
    forward(backing_width)
    right(90) # face south
    forward(backing_height)
    right(90) # face west
    forward(backing_width)
    end_fill()
 
    # Inner rectangle
    penup()
    goto(- backing_width // 2 + backing_margin,
         - backing_height // 2 + backing_margin) # bottom left
    fillcolor('gainsboro')
    begin_fill()
    pendown()
    setheading(90) # face north
    forward(backing_height - backing_margin * 2)
    right(90) # face east
    forward(backing_width - backing_margin * 2)
    right(90) # face south
    forward(backing_height - backing_margin * 2)
    right(90) # face west
    forward(backing_width - backing_margin * 2)
    end_fill()
 
    # Draw lines separating the locations where the sheets go
    width(1); pencolor('dim grey')
    for horizontal in [-sheet_width, 0, sheet_width]:
        penup()
        goto(horizontal, sheet_height // 2)
        pendown()
        setheading(270) # point south
        forward(sheet_height)
          
    # Mark the centre points of each sheet's location, if desired
    if mark_centre_points:
        penup()
        points = [[[round(-sheet_width * 1.5), 0], 'Location 1'],
                  [[round(-sheet_width * 0.5), 0], 'Location 2'],
                  [[round(sheet_width * 0.5), 0], 'Location 3'],
                  [[round(sheet_width * 1.5), 0], 'Location 4']]
        for centre_point, label in points:
            goto(centre_point)
            dot(4)
            write('  ' + label + '\n  (' + str(centre_point[0]) + ', 0)',
                  font = ('Arial', 12, 'normal'))
      
    # Reset everything ready for the student's solution
    color('black')
    width(1)
    penup()
    home()
    setheading(0)
    tracer(True)
 
 
# End the program by hiding the cursor and releasing the canvas
def release_drawing_canvas():
    tracer(True)
    hideturtle()
    done()
     
#
#--------------------------------------------------------------------#
 
 
 
#-----Test Data------------------------------------------------------#
#
# The list in this section contains the data sets you will use to
# test your code.  Each of the data sets is a list specifying the
# way in which sheets are pasted onto the billboard:
#
# 1. The name of the sheet, from 'Sheet A' to 'Sheet D'
# 2. The location to paste the sheet, from 'Location 1' to
#    'Location 4'
# 3. The sheet's orientation, either 'Upright' or 'Upside down'
#
# Each data set does not necessarily mention all four sheets.
#
# In addition there is an extra value, either 'X' or 'O' at the
# start of each data set.  The purpose of this value will be
# revealed only in Part B of the assignment.  You should ignore it
# while completing Part A.
#
# You can create further data sets, but do not change any of the
# given ones below because they will be used to test your submission.
#
# Note that your solution must work for all the data sets below
# AND ANY OTHER DATA SETS IN THE SAME FORMAT!
#
 
data_sets = [
    # These two initial data sets don't put any sheets on the billboard
    # Data sets 0 - 1
    ['O'],
    ['X'],
    # These data sets put Sheet A in all possible locations and orientations
    # Data sets 2 - 9
    ['O', ['Sheet A', 'Location 1', 'Upright']],
    ['O', ['Sheet A', 'Location 2', 'Upright']],
    ['O', ['Sheet A', 'Location 3', 'Upright']],
    ['O', ['Sheet A', 'Location 4', 'Upright']],
    ['O', ['Sheet A', 'Location 1', 'Upside down']],
    ['O', ['Sheet A', 'Location 2', 'Upside down']],
    ['O', ['Sheet A', 'Location 3', 'Upside down']],
    ['O', ['Sheet A', 'Location 4', 'Upside down']],
    # These data sets put Sheet B in all possible locations and orientations
    # Data sets 10 - 17
    ['O', ['Sheet B', 'Location 1', 'Upright']],
    ['O', ['Sheet B', 'Location 2', 'Upright']],
    ['O', ['Sheet B', 'Location 3', 'Upright']],
    ['O', ['Sheet B', 'Location 4', 'Upright']],
    ['O', ['Sheet B', 'Location 1', 'Upside down']],
    ['O', ['Sheet B', 'Location 2', 'Upside down']],
    ['O', ['Sheet B', 'Location 3', 'Upside down']],
    ['O', ['Sheet B', 'Location 4', 'Upside down']],
    # These data sets put Sheet C in all possible locations and orientations
    # Data sets 18 - 25
    ['O', ['Sheet C', 'Location 1', 'Upright']],
    ['O', ['Sheet C', 'Location 2', 'Upright']],
    ['O', ['Sheet C', 'Location 3', 'Upright']],
    ['O', ['Sheet C', 'Location 4', 'Upright']],
    ['O', ['Sheet C', 'Location 1', 'Upside down']],
    ['O', ['Sheet C', 'Location 2', 'Upside down']],
    ['O', ['Sheet C', 'Location 3', 'Upside down']],
    ['O', ['Sheet C', 'Location 4', 'Upside down']],
    # These data sets put Sheet D in all possible locations and orientations
    # Data sets 26 - 33
    ['O', ['Sheet D', 'Location 1', 'Upright']],
    ['O', ['Sheet D', 'Location 2', 'Upright']],
    ['O', ['Sheet D', 'Location 3', 'Upright']],
    ['O', ['Sheet D', 'Location 4', 'Upright']],
    ['O', ['Sheet D', 'Location 1', 'Upside down']],
    ['O', ['Sheet D', 'Location 2', 'Upside down']],
    ['O', ['Sheet D', 'Location 3', 'Upside down']],
    ['O', ['Sheet D', 'Location 4', 'Upside down']],
    # These data sets place two sheets in various locations and orientations
    # Data sets 34 - 38
    ['O', ['Sheet D', 'Location 2', 'Upright'],
          ['Sheet C', 'Location 4', 'Upright']],
    ['O', ['Sheet A', 'Location 3', 'Upright'],
          ['Sheet B', 'Location 1', 'Upright']],
    ['O', ['Sheet D', 'Location 1', 'Upside down'],
          ['Sheet C', 'Location 4', 'Upright']],
    ['O', ['Sheet A', 'Location 3', 'Upright'],
          ['Sheet B', 'Location 2', 'Upside down']],
    ['X', ['Sheet C', 'Location 1', 'Upright'],
          ['Sheet B', 'Location 2', 'Upside down']],
    # These data sets place three sheets in various locations and orientations
    # Data sets 39 - 43
    ['O', ['Sheet A', 'Location 4', 'Upright'],
          ['Sheet B', 'Location 3', 'Upright'],
          ['Sheet C', 'Location 2', 'Upright']],
    ['O', ['Sheet C', 'Location 1', 'Upright'],
          ['Sheet A', 'Location 3', 'Upright'],
          ['Sheet D', 'Location 4', 'Upright']],
    ['O', ['Sheet C', 'Location 1', 'Upside down'],
          ['Sheet D', 'Location 3', 'Upside down'],
          ['Sheet A', 'Location 4', 'Upright']], 
    ['O', ['Sheet B', 'Location 4', 'Upright'],
          ['Sheet D', 'Location 2', 'Upside down'],
          ['Sheet C', 'Location 1', 'Upside down']], 
    ['X', ['Sheet A', 'Location 4', 'Upright'],
          ['Sheet D', 'Location 3', 'Upside down'],
          ['Sheet C', 'Location 2', 'Upright']],
    # These data sets place four sheets in various locations and orientations
    # Data sets 44 - 48
    ['O', ['Sheet C', 'Location 1', 'Upright'],
          ['Sheet B', 'Location 2', 'Upright'],
          ['Sheet A', 'Location 3', 'Upright'],
          ['Sheet D', 'Location 4', 'Upright']],
    ['O', ['Sheet C', 'Location 2', 'Upright'],
          ['Sheet B', 'Location 3', 'Upright'],
          ['Sheet D', 'Location 1', 'Upright'],
          ['Sheet A', 'Location 4', 'Upright']],
    ['O', ['Sheet C', 'Location 1', 'Upside down'],
          ['Sheet B', 'Location 2', 'Upright'],
          ['Sheet A', 'Location 3', 'Upright'],
          ['Sheet D', 'Location 4', 'Upside down']],
    ['O', ['Sheet C', 'Location 2', 'Upright'],
          ['Sheet B', 'Location 3', 'Upside down'],
          ['Sheet D', 'Location 1', 'Upside down'],
          ['Sheet A', 'Location 4', 'Upright']],
    ['X', ['Sheet C', 'Location 1', 'Upright'],
          ['Sheet B', 'Location 2', 'Upside down'],
          ['Sheet A', 'Location 3', 'Upright'],
          ['Sheet D', 'Location 4', 'Upside down']],     
    # These data sets draw the entire image upside down
    # Data sets 49 - 50
    ['X', ['Sheet A', 'Location 4', 'Upside down'],
          ['Sheet B', 'Location 3', 'Upside down'],
          ['Sheet C', 'Location 2', 'Upside down'],
          ['Sheet D', 'Location 1', 'Upside down']],
    ['O', ['Sheet A', 'Location 4', 'Upside down'],
          ['Sheet B', 'Location 3', 'Upside down'],
          ['Sheet C', 'Location 2', 'Upside down'],
          ['Sheet D', 'Location 1', 'Upside down']],
    # These are the final, 'correct' arrangements of sheets
    # Data sets 51 - 52
    ['X', ['Sheet A', 'Location 1', 'Upright'],
          ['Sheet B', 'Location 2', 'Upright'],
          ['Sheet C', 'Location 3', 'Upright'],
          ['Sheet D', 'Location 4', 'Upright']],
    ['O', ['Sheet A', 'Location 1', 'Upright'],
          ['Sheet B', 'Location 2', 'Upright'],
          ['Sheet C', 'Location 3', 'Upright'],
          ['Sheet D', 'Location 4', 'Upright']]
    ]
 
#
#--------------------------------------------------------------------#
 
 
 
#-----Student's Solution---------------------------------------------#
#
#  Complete the assignment by replacing the dummy function below with
#  your own "paste_up" function.
#
 
# Paste the sheets onto the billboard as per the provided data set
def paste_up(dummy_parameter):
    #fill background color to white
    if len(dummy_parameter) >= 2:
        # set the background to white
        sheets = dummy_parameter[1:]
        for sheet in sheets:
            name = sheet[0]
            location = sheet[1]
            direction = sheet[2]
            y = 0
            if location == "Location 1":
                x = -300
            elif location == "Location 2":
                x = -100
            elif location == "Location 3":
                x = 100
            else:
                x = 300
            penup()
            goto(x-sheet_width/2-1, y-sheet_height/2+1) # bottom left
            pencolor("white")
            width(0)
            fillcolor('white')
            begin_fill()
            pendown()
            setheading(90) # face north
            forward(sheet_height-1)
            right(90) # face east
            forward(sheet_width-1)
            right(90) # face south
            forward(sheet_height-1)
            right(90) # face west
            forward(sheet_width-1)
            end_fill()
 
            if name == "Sheet A":
                #draw char e
                x = 0
                y = 0
                if location == "Location 1":
                    x = -300
                elif location == "Location 2":
                    x = -100
                elif location == "Location 3":
                    x = 100
                else:
                    x = 300
                if direction == "Upright":
                    penup()
                    goto(x, y)
                    setheading(90)
                    color('Crimson')
                    dot(190)
                    color("white")
                    dot(140)
                    penup()
                    goto(x-70, y)
                    color("crimson")
                    width(20)
                    pendown()
                    setheading(0)
                    forward(140)
                    penup()
                    goto(x+30, y-10)
                    fillcolor('white')
                    begin_fill()
                    setheading(0) # face north
                    forward(65)
                    right(90) # face east
                    forward(30)
                    right(90) # face south
                    forward(60)
                    end_fill()
                else:
                    penup()
                    goto(x, y)
                    setheading(270)
                    color('crimson')
                    dot(190)
                    color("white")
                    dot(140)
                    penup()
                    goto(x+70, y)
                    color("Crimson")
                    width(20)
                    pendown()
                    setheading(180)
                    forward(140)
                    penup()
                    goto(x-30, y+10)
                    fillcolor('white')
                    begin_fill()
                    setheading(180) # face north
                    forward(65)
                    right(90) # face east
                    forward(30)
                    right(90) # face south
                    forward(60)
                    end_fill()
 
            if name == "Sheet B":
                # draw char b
                x = 0
                y = 0
                if location == "Location 1":
                    x = -300 + 10
                elif location == "Location 2":
                    x = -100 + 10
                elif location == "Location 3":
                    x = 100 + 10
                else:
                    x = 300 + 10
                if direction == "Upright":
                    penup()
                    goto(x, y)
                    color("blue")
                    dot(180)
                    goto(x+5,y)
                    color('white')
                    dot(140)
                    # draw vertical line
                    goto(x-90, y-80)
                    width(1)
                    fillcolor('blue')
                    begin_fill()
                    setheading(90) # face north
                    forward(250)
                    right(90) # face east
                    forward(25)
                    right(90) # face south
                    forward(250)
                    end_fill()
                else:
                    penup()
                    goto(x, y)
                    color("blue")
                    dot(180)
                    goto(x-5,y)
                    color('white')
                    dot(140)
                    # draw vertical line
                    goto(x+90, y+80)
                    width(1)
                    fillcolor('blue')
                    begin_fill()
                    setheading(270) # face north
                    forward(250)
                    right(90) # face east
                    forward(25)
                    right(90) # face south
                    forward(250)
                    end_fill()
 
            if name == "Sheet C":
                #**************draw char a*****************
                x = 0
                y = 0
                if location == "Location 1":
                    x = -300
                elif location == "Location 2":
                    x = -100
                elif location == "Location 3":
                    x = 100
                else:
                    x = 300
                if direction == "Upright":
                    penup()
                    color("Gold")
                    goto(x, y+20)
                    pendown()
                    dot(160)
                    color("white")
                    dot(120)
                    penup()
                    goto(x-100,y+40)
                    setheading(0)
                    fillcolor('white')
                    begin_fill()
                    forward(180)
                    right(90)
                    forward(120)
                    right(90)
                    forward(180)
                    end_fill()
                    penup()
                    goto(x+65, y+44)
                    pencolor("Gold")
                    width(20)
                    pendown()
                    setheading(-90)
                    forward(120)
                    penup()
                    goto(x-20,y)
                    setheading(180)
                    pendown()
                    circle(40, 180)
                    penup()
                    goto(x-20,y)
                    pendown()
                    setheading(0)
                    forward(80)
                    penup()
                    goto(x-20, y-80)
                    pendown()
                    setheading(0)
                    circle(80, 90)
                else:
                    penup()
                    color("Gold")
                    goto(x, y-20)
                    pendown()
                    dot(160)
                    color("white")
                    dot(120)
                    penup()
                    goto(x+100,y-40)
                    setheading(180)
                    fillcolor('white')
                    begin_fill()
                    forward(200)
                    right(90)
                    forward(120)
                    right(90)
                    forward(200)
                    end_fill()
                    penup()
                    goto(x-65, y-44)
                    pencolor("Gold")
                    width(20)
                    pendown()
                    setheading(90)
                    forward(120)
                    penup()
                    goto(x+20,y)
                    setheading(0)
                    pendown()
                    circle(40, 180)
                    penup()
                    goto(x+20,y)
                    pendown()
                    setheading(180)
                    forward(80)
                    penup()
                    goto(x+20, y+80)
                    pendown()
                    setheading(180)
                    circle(80, 90)
 
            if name == "Sheet D":
                # draw char y
                x = 0
                y = 0
                if location == "Location 1":
                    x = -300
                elif location == "Location 2":
                    x = -100
                elif location == "Location 3":
                    x = 100
                else:
                    x = 300
                if direction == "Upright":
                    penup()
                    goto(x-90,y+80)
                    width(20)
                    pencolor("green")
                    pendown()
                    setheading(-60)
                    forward(170)
                    penup()
                    goto(x+90,y+80)
                    pendown()
                    setheading(-120)
                    forward(280)
                else:
                    penup()
                    goto(x+90,y-80)
                    width(20)
                    pencolor("green")
                    pendown()
                    setheading(120)
                    forward(170)
                    penup()
                    goto(x-90,y-80)
                    pendown()
                    setheading(60)
                    forward(280)
 
 
#
#--------------------------------------------------------------------#
 
 
 
#-----Main Program---------------------------------------------------#
#
# This main program sets up the background, ready for you to start
# drawing your billboard.  Do not change any of this code except
# where indicated by comments marked '*****'.
#
 
# Set up the drawing canvas
# ***** Change the default argument to False if you don't want to
# ***** display the centre points of each sheet on the backing
create_drawing_canvas()
 
# sheet A-Location 1
#turtle.position(-300,0)
 
 
# Control the drawing speed
# ***** Modify the following argument if you want to adjust
# ***** the drawing speed
speed('fastest')
 
# Decide whether or not to show the drawing being done step-by-step
# ***** Set the following argument to False if you don't want to wait
# ***** while the cursor moves around the screen
tracer(True)
 
# Give the drawing canvas a title
# ***** Replace this title with one that describes the image
# ***** displayed on your billboard when the sheets are pasted
# ***** correctly
title("Ebay")
 
### Call the student's function to display the billboard
### ***** Change the number in the argument to this function
### ***** to test your code with a different data set
paste_up(data_sets[2])
 
# Exit gracefully
release_drawing_canvas()
 
#
#--------------------------------------------------------------------#

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
