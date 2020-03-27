---
title: turtle example londonWind (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'londonWind'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`
* `import csv`
* `import cairosvg`
* `import shutil`
* `import tempfile`
* `import os`
* `import turtle`

## python londonWind

Python turtle example: londonWind

```python
#!/usr/bin/python
"""     
Make a map of where a turtle would be pushed each day
by the wind in a certain location
Where the wind would push you in 2015 London. Power and direction are taken into account each day at noon. Code and data is at https://gist.github.com/cavedave/6b3862936fefea14c241ea8537c4053d

"""
from tkinter import *
from turtle import *
import turtle
import os
import tempfile
import shutil
import cairosvg
from canvasvg import canvasvg
import csv
import pandas as pd
import numpy as np


silly = turtle.Turtle()


silly.right(270)#start pointing north. turtle starts pointing ->
silly.pensize(2)
df = pd.read_csv('london2015d.csv')
#print(df)
silly.hideturtle()
silly.speed(0)
silly.penup()
silly.goto(-450,-330)
silly.pendown()

#write in the start day
silly.penup()
silly.write("Jan 1st", True, align="left",font=("Arial", 15, "normal"))
silly.pendown()
for index, row in df.iterrows():
    #ddhm		Mean wind direction over 10 minutes at time of highest mean
	#point in a direction opposite of the wind angle. they say where it comes from.
	go=row['WD50m_deg']+180

	silly.right(go)
	silly.forward((row['WS50m_m/s']))
	#we want to point back at the start for next days wind
	backN=360- go
	silly.right(backN)
    #wdsp		Mean Wind Speed
	if index ==182:
		silly.penup()
		silly.write("Jul 1st", True, align="left",font=("Arial", 15, "normal"))
		silly.pendown()


#write in the last day
silly.penup()
silly.write("Dec 31st", True, align="left",font=("Arial", 15, "normal"))
silly.penup()

# Make a header
silly.penup()
silly.goto(-140,120)
silly.write("London Wind Map for 2015", True, align="left",font=("Arial", 25, "normal"))
silly.penup()
# Make a compass
silly.penup()
silly.goto(670,-320)
silly.write("⇑N", True, align="left",font=("Arial", 30, "normal"))


silly.penup()

silly.goto(-140,80)
silly.write("If you were pushed by the wind each day\nthis is the path you'd take", True, align="left",font=("Arial", 15, "normal"))
silly.penup()
silly.goto(0,-350)
silly.pendown()

# get image
ts= silly.getscreen()
ts.getcanvas().postscript(file="london.eps")

#finish up
turtle.done()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
