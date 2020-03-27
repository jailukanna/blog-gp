---
title: turtle example PyHW TwoCanvases 3 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'PyHW TwoCanvases 3'

Functions in program: 
* `def main():`

## python PyHW TwoCanvases 3

Python turtle example: PyHW TwoCanvases 3

```python
"""
Turtledemo.two_canvases
牟光邦
M0329009
"""

from turtle import TurtleScreen, RawTurtle, TK

def main():
    root = TK.Tk()
    cv1 = TK.Canvas(root, width=300, height=300, bg="#ddffff") #window(up) information
    cv2 = TK.Canvas(root, width=300, height=300, bg="#ffeeee") #window(down) information
    cv1.pack()
    cv2.pack()

    s1 = TurtleScreen(cv1)
    s1.bgcolor(0.85, 0.85, 1)
    s2 = TurtleScreen(cv2)
    s2.bgcolor(1, 0.85, 0.85)

    p = RawTurtle(s1)
    q = RawTurtle(s2)

    p.color("red", (0, 0.5, 0.5))  #Turtle colot,Background color(inner)
    p.width(3)  #Line Size
    q.color("blue", (0.5, 0.5, 0))
    q.width(3)

    for t in p,q:
        t.shape("turtle")
        t.lt(60)

    q.lt(180)  #Up Turtle Rotation about 180

    for t in p, q:
        t.begin_fill()
    for i in range(3):  #Number of Draw
        for t in p, q:
            t.fd(45)  #Line Long
            t.lt(120)  #Turtle Angle
    for t in p,q:
        t.end_fill()
        t.lt(30)
        t.pu()
        t.bk(50)

    
    return "EVENTLOOP"


if __name__ == '__main__':
    main()
    TK.mainloop()  # keep window open until user closes it

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
