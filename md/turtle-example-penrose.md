---
title: turtle example penrose (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'penrose'

Functions in program: 
* `def main():`
* `def demo(fun=sun):`
* `def test(l=200, n=4, fun=sun, startpos=(0,0), th=2):`
* `def start():`
* `def makeshapes():`
* `def star(l,n):`
* `def sun(l, n):`
* `def draw(l, n, th=2):`
* `def inflatedart(l, n):`
* `def inflatefish(l, n):`
* `def dart(l):		#function declaration`
* `def bgAlternate(self, event):`
* `def grayBg():`
* `def greenBg():`
* `def fish(l):		#function declaration`

## python penrose

Python turtle example: penrose

```python
"""       xturtle_fish_and_darts.py
Constructs two aperiodic penrose-tilings,
consisting of fish and darts, by the method
of inflation in six steps.
 -------------------------------------------
    By : Apriandy Angdresey (M0329020)
"""

from turtle import *  #turtle graphic library
from math import cos, pi
from time import clock, sleep
scr = Screen()

"""
variable global
"""
f = (5**0.5-1)/2.0   # (sqrt(5)-1)/2 -- golden ratio
d = 2 * cos(3*pi/10)

"""
this function to create a shape fish
"""
def fish(l):		#function declaration
    fl = f * l
    lt(45)		#turtle.left() function of angle in turtle
    fd(l)		#turtle.foward()
    rt(135)		##turtle.right() function of angle in turtle
    fd(fl)
    rt(45)
    fd(fl)
    rt(135)
    fd(l)
    rt(180)

def greenBg():
    bgcolor("green");
    
def grayBg():
    bgcolor("gray");
    
def bgAlternate(self, event):
    bg = bgcolor();
    if bg == "green":
        grayBg()
    else:
        greenBg()

"""
this function to create a shape dart
"""
def dart(l):		#function declaration
    fl = f * l
    lt(36)		#turtle.left() function of angle in turtle
    fd(l)		#turtle.foward()
    rt(144)		##turtle.right() function of angle in turtle
    fd(fl)
    lt(36)
    fd(fl)
    rt(144)
    fd(l)
    rt(144)

"""
this function to make inflate the shape fish
"""
def inflatefish(l, n):
    if n == 0:
        px, py = pos()			#the turtle position x,y and Vector 2D
        h, x, y = int(heading()), round(px,3), round(py,3)
        tiledict[(h,x,y)] = True
        return
    fl = f * l
    lt(36)
    inflatedart(fl, n-1)			#}recursive => call the function within itself
    fd(l)			   		#}
    rt(144)			   		#}
    inflatefish(fl, n-1)			#}
    lt(18)
    fd(l*d)
    rt(162)			
    inflatefish(fl, n-1)			#}recursive => call the function within itself
    lt(36)			   		#}
    fd(l)			   		#}
    rt(180)			   		#}
    inflatedart(fl, n-1)			#}
    lt(36)

"""
this function to make inflate the shape dart
"""
def inflatedart(l, n):
    if n == 0:
        px, py = pos()
        h, x, y = int(heading()), round(px,3), round(py,3)
        tiledict[(h,x,y)] = False
        return
    fl = f * l
    inflatefish(fl, n-1)
    lt(36)
    fd(l)
    rt(180)
    inflatedart(fl, n-1)
    lt(54)
    fd(l*d)
    rt(126)
    inflatedart(fl, n-1)
    fd(l)
    rt(144)

"""
function to draw
"""
def draw(l, n, th=2):
    clear()					#to clear the images on the screen turtle
    l = l * f**n
    shapesize(l/100.0, l/100.0, th)		#set the size of the pen/point
    for k in tiledict:
        h, x, y = k
        setpos(x, y)				 #move the pen/point to a certain position
        setheading(h)				 #set the pen/point orientation by a certain angle
        if tiledict[k]:
            shape("fish")			 #call a shape with a certain name from library
            color("black", ("yellow")) 	#colour
        else:
            shape("dart")			 #call a shape with a certain name from library
            color("white", ("red"))	        #colour
        stamp()					 #copying shape

"""
make the shape sun combined with flat figure fish
"""
def sun(l, n):
    for i in range(5):
        inflatefish(l, n)
        lt(72)

"""
make the shape star combined with flat figure dart
"""
def star(l,n):
    for i in range(5):
        inflatedart(l, n)
        lt(72)

def makeshapes():
    tracer(0)
    begin_poly()		        #record/saving the shape to library
    fish(100)
    end_poly()
    register_shape("fish", get_poly())	#add a new shape to turtle library
    begin_poly()
    dart(100)
    end_poly()
    register_shape("dart", get_poly())
    tracer(1)

def start():
    reset()
    ht()
    pu()
    makeshapes()
    resizemode("user")

"""
The main blocks, the image is displayed in full screen
"""
def test(l=200, n=4, fun=sun, startpos=(0,0), th=2):
    global tiledict
    goto(startpos)			#same with setpos()
    setheading(0)
    tiledict = {}
    a = clock()
    tracer(0)
    fun(l, n)
    b = clock()
    draw(l, n, th)
    tracer(1)
    c = clock()

    print("Calculation:   %7.4f s" % (b - a))
    print("Drawing:  %7.4f s" % (c - b))
    print("Together: %7.4f s" % (c - a))
    nk = len([x for x in tiledict if tiledict[x]])
    nd = len([x for x in tiledict if not tiledict[x]])
    print("%d fish and %d darts = %d pieces." % (nk, nd, nk+nd))

"""
demo image slowly as well add "Kite" and "dart" to turtle shape library
"""
def demo(fun=sun):
    start()				#starting point
    for i in range(8):
        a = clock()
        test(300, i, fun)
        b = clock()
        t = b - a
        if t < 2:
            sleep(2 - t)


def main():
    greenBg()
    fish(100)
    scr.onclick(bgAlternate)
    
    #title("Penrose-tiling with fish and darts.") //window title
    mode("logo")
    demo(sun)
    sleep(2)		        #delay program during the time in the parameters
    demo(star)
    pencolor("black")			#}initialization turtle canvas
    goto(0,-380)			#}
    pencolor("green")			#}
    write("Please wait", align="center", font=('Arial', 35, 'bold'))
    test(600, 8, startpos=(70, 117))	#make Penrose as screen size.
    return "Done"

if __name__ == "__main__":
    msg = main()
    mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
