---
title: turtle example grav (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'grav'

Functions in program: 
* `def exity(x,y):`
* `def step():`
* `def degToRad(deg):`
* `def radToDeg(rad):`

Modules used in program: 
* `import math`

## python grav

Python turtle example: grav

```python
import math
from turtle import Turtle

def radToDeg(rad):
    return rad / math.pi * 180

def degToRad(deg):
    return deg * math.pi / 180

class Orbiter():
    def __init__(self, xpos, ypos, mass, vel):
        self.x = xpos
        self.y = ypos
        self.v = vel
        self.m = mass

        # Calculated later
        self.a = Vector(0,0)
        self.f = Vector(0,0)

        # For visualization
        self.ftheta = 0
        self.turtle = Turtle()
        self.turtle.speed(0)
        self.turtle.pu()
        self.turtle.goto(50*self.x, 50*self.y)
        self.turtle.pd()

        self.turtle.color((xpos%5)/5, (ypos%5)/5, ((xpos+ypos)%5)/5)
        s = self.m ** .5
        self.turtle.turtlesize(stretch_wid=s, stretch_len=s)
        self.turtle.getscreen().bgcolor(.2,.2,.2)

    def moveGraphic(self):
        force = self.f
        self.turtle.goto(50*self.x, 50*self.y)
        self.ftheta = -1*force.argument() - 90
        self.turtle.seth(self.ftheta)

""" 
License for Vector class (rest of code is my own):
The MIT License (MIT)
Copyright (c) 2015 Mat Leonard
Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
"""
class Vector(object):
    def __init__(self, *args):
        """ Create a vector, example: v = Vector(1,2) """
        if len(args)==0: self.values = (0,0)
        else: self.values = args
        
    def norm(self):
        """ Returns the norm (length, magnitude) of the vector """
        return math.sqrt(sum( comp**2 for comp in self ))
        
    def argument(self):
        """ Returns the argument of the vector, the angle clockwise from +y."""
        arg_in_rad = math.acos(Vector(0,1)*self/self.norm())
        arg_in_deg = math.degrees(arg_in_rad)
        if self.values[0]<0: return 360 - arg_in_deg
        else: return arg_in_deg

    def normalize(self):
        """ Returns a normalized unit vector """
        norm = self.norm()
        normed = tuple( comp/norm for comp in self )
        return Vector(*normed)
    
    def rotate(self, *args):
        """ Rotate this vector. If passed a number, assumes this is a 
            2D vector and rotates by the passed value in degrees.  Otherwise,
            assumes the passed value is a list acting as a matrix which rotates the vector.
        """
        if len(args)==1 and type(args[0]) == type(1) or type(args[0]) == type(1.):
            # So, if rotate is passed an int or a float...
            if len(self) != 2:
                raise ValueError("Rotation axis not defined for greater than 2D vector")
            return self._rotate2D(*args)
        elif len(args)==1:
            matrix = args[0]
            if not all(len(row) == len(v) for row in matrix) or not len(matrix)==len(self):
                raise ValueError("Rotation matrix must be square and same dimensions as vector")
            return self.matrix_mult(matrix)
        
    def _rotate2D(self, theta):
        """ Rotate this vector by theta in degrees.
            
            Returns a new vector.
        """
        theta = math.radians(theta)
        # Just applying the 2D rotation matrix
        dc, ds = math.cos(theta), math.sin(theta)
        x, y = self.values
        x, y = dc*x - ds*y, ds*x + dc*y
        return Vector(x, y)
        
    def matrix_mult(self, matrix):
        """ Multiply this vector by a matrix.  Assuming matrix is a list of lists.
        
            Example:
            mat = [[1,2,3],[-1,0,1],[3,4,5]]
            Vector(1,2,3).matrix_mult(mat) ->  (14, 2, 26)
         
        """
        if not all(len(row) == len(self) for row in matrix):
            raise ValueError('Matrix must match vector dimensions') 
        
        # Grab a row from the matrix, make it a Vector, take the dot product, 
        # and store it as the first component
        product = tuple(Vector(*row)*self for row in matrix)
        
        return Vector(*product)

    def y(self):
        theta = -1 * self.argument() - 90 # argument() returns an angle clockwise from +y
        return self.norm() * math.sin(degToRad(theta))
    
    def x(self):
        theta = -1 * self.argument() - 90 # argument() returns an angle clockwise from +y
        return self.norm() * math.cos(degToRad(theta))
    
    def inner(self, other):
        """ Returns the dot product (inner product) of self and other vector
        """
        return sum(a * b for a, b in zip(self, other))
    
    def __mul__(self, other):
        """ Returns the dot product of self and other if multiplied
            by another Vector.  If multiplied by an int or float,
            multiplies each component by other.
        """
        if type(other) == type(self):
            return self.inner(other)
        elif type(other) == type(1) or type(other) == type(1.0):
            product = tuple( a * other for a in self )
            return Vector(*product)
    
    def __rmul__(self, other):
        """ Called if 4*self for instance """
        return self.__mul__(other)
            
    def __div__(self, other):
        if type(other) == type(1) or type(other) == type(1.0):
            divided = tuple( a / other for a in self )
            return Vector(*divided)
    
    def __add__(self, other):
        """ Returns the vector addition of self and other """
        added = tuple( a + b for a, b in zip(self, other) )
        return Vector(*added)
    
    def __sub__(self, other):
        """ Returns the vector difference of self and other """
        subbed = tuple( a - b for a, b in zip(self, other) )
        return Vector(*subbed)
    
    def __iter__(self):
        return self.values.__iter__()
    
    def __len__(self):
        return len(self.values)
    
    def __getitem__(self, key):
        return self.values[key]
        
    def __repr__(self):
        return str(self.values)

# Define our objects to orbit.
# Parameters are x, y, mass, and initial velocity (a vector)
a = Orbiter(2, 2, 2.0, Vector(4, -4))
b = Orbiter(2, -2, 2.0, Vector(-4, -4))
c = Orbiter(-2, -2, 2.0, Vector(-4, 4))
d = Orbiter(-2, 2, 2.0, Vector(4, 4))

g = Orbiter(0, 3, .10, Vector(.56, 0))
h = Orbiter(0, -3, .10, Vector(-.56, 0))

e = Orbiter(20, 2, 3.0, Vector(2, 2))
f = Orbiter(-20, -2, 3.0, Vector(-2, -2))

# make nice list of bodies to consider
bodies = [a,b,c,d,e,f,g,h]

# Time step interval (smaller = higher resolution)
dt = .009

# Universal gravitational constant
G = 40


def step():
    for body in bodies:
        body.f = Vector(0,0)
        n = len(bodies)
        for i in range(n):
            other = bodies[i]
            if other != body:
                toOther = Vector(body.x-other.x, body.y-other.y)
                dist = toOther.norm()
                body.f += G * body.m * other.m * 1/(dist**2) * toOther.normalize()


                #if dist < .1:
                # An attempt to simulate collisions.  What's tried here is splitting each body into two, and reversing direction.
                # However, it doesn't work very well, so it's best left out.
                if False:
                    body.v = -.5 * body.v
                    body.m /= 2
                    offset = .11 * toOther.normalize()
                    body.v += offset.rotate(90)
                    body.x -= offset.x()
                    body.y -= offset.y()
                    bodies.append(Orbiter(body.x+offset.x(), body.y+offset.y(), body.m, body.v + offset.rotate(90)))

    for body in bodies:
        body.a = 1/body.m * body.f 
        body.v += dt*body.a
        body.x += dt*body.v.x()
        body.y += dt*body.v.y()
        body.moveGraphic()

                


def exity(x,y):
    a.turtle.getscreen().bye()
    print("Exited")

try:
    while True:
        step()
except KeyboardInterrupt:
    exity(None, None)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
