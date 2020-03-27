---
title: turtle example pi-tau-euler (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'pi-tau-euler'

Functions in program: 
* `def main():`
* `def piArt(digits, canvas, color_list, square_size, initial_angle, hex_dec):`
* `def readDecimals(decimalfile, hex_dec):`
* `def drawSquare(square_size, square_color, canvas):`

Modules used in program: 
* `import os`
* `import sys`
* `import time`

## python pi-tau-euler

Python turtle example: pi-tau-euler

```python
#!/usr/bin/env python
#-*- coding:utf-8 -*-
import time
import sys
import os
from turtle import *

'''
    pi-tau-art.py - Copyright 2019 Ørjan Hoyd H. Vøllestad <hoyd@earth>
    
    SCRIPT TO DRAW Pi AND Tau ART FROM THEIR DECIMALS USING TURTLE
    
    For each decimal in pi or tau, draw a pixel square with the distinct color 
    matching the decimal value:

    #e6194b - 0
    #f58231 - 1
    #ffe119 - 2
    #bfef45 - 3
    #3cb44b - 4
    #42d4f4 - 5
    #4363d8 - 6
    #911eb4 - 7
    #f032e6 - 8
    #a9a9a9 - 9

    Then, move forward two times the value and turn left some degrees 
    also times the value of the decimal.
'''

def drawSquare(square_size, square_color, canvas):
    ''' The function used to draw the square and fill with color
    '''
    canvas.color(square_color)
    canvas.pendown()
    canvas.begin_fill()
    for line in range(4):
        canvas.left(90)
        canvas.forward(square_size)
    canvas.end_fill()
    canvas.penup()
    
def readDecimals(decimalfile, hex_dec):
    ''' Open textfile with pi digits and create a list of the digits
    '''
    with open(decimalfile, 'r') as file:
        data = file.read().replace('\n', '')
    pi_digits = [str(d) for d in str(data)]
    # ~ print(pi_digits)
    return pi_digits

def piArt(digits, canvas, color_list, square_size, initial_angle, hex_dec):
    ''' A function to call the square drawing, forward and turn according
        to the decimal value.
    '''
    
    canvas.penup()
    # Move to a good starting point before drawing
    canvas.goto(1500,-2000) # good position for pi100k
    # ~ canvas.goto(2500,-2000) # good position for pi10k
    # ~ canvas.goto(500,-1800) # good position for euler10k
    # ~ canvas.goto(2000,-3 000) # good position for tau100k
    for idx, val in enumerate(digits):
        # ~ posx,posy = canvas.pos()
        drawSquare(square_size, str(color_list[int(str(val), int(hex_dec))]), canvas)
        canvas.forward(square_size*int(str(val), int(hex_dec)))
        canvas.left(initial_angle*int(str(val), int(hex_dec)))

def main():
    ''' The main function where some variables are set and the drawing starts
    '''
    # Set some variables for the canvas and drawing
    if len(sys.argv) < 3:
        exit()

    if sys.argv[2] == '10':            
        hex_colors = ['#e6194b', '#f58231', '#ffe119', '#bfef45', '#3cb44b', \
            '#42d4f4', '#4363d8', '#911eb4', '#f032e6', '#a9a9a9']
        hex_dec = 10
    elif sys.argv[2] == '16':
        hex_colors = ['#e6194b', '#3cb44b', '#ffe119', '#4363d8', '#f58231', \
            '#911eb4', '#46f0f0', '#f032e6', '#bcf60c', '#008080', '#9a6324', \
            '#800000', '#808000', '#000075', '#808080', '#000000']
        hex_dec = 16
    else:
        print("no number base set\n")
        exit(1)

    eps_filename = os.path.splitext(sys.argv[2])[0]
    canvas_width = 5000
    canvas_height = 5000
    square_size = 2
    initial_angle = 90

    # DO SOME TURTLE SETUP
    t = Turtle() # Set up a turtle instance with a screen and a canvas
    t.screen.setup(canvas_width, canvas_height) # Setting up the window "screen" size
    t.screen.screensize(canvas_width, canvas_height, 'White') # Not needed for the canvas output, just for viewing
    t.hideturtle() # Don't show the turtle on screen
    t.tracer(0) # Make turtle draw instant

    # Do some plotting
    piArt(readDecimals(sys.argv[1], hex_dec), t, hex_colors, square_size, initial_angle, hex_dec)

    # Save the art as an image and big enough
    filename = eps_filename + "_plotted_" + time.strftime("%Y-%m-%d_%H-%M-%S") + ".eps"
    getcanvas().postscript(file=filename, colormode='color', width=canvas_width, height=canvas_height)

    exitonclick() # Close the drawing window when tapping or clicking on it

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
