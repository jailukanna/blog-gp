---
title: turtle example Quadratics (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'Quadratics'

Functions in program: 
* `def main():`

Modules used in program: 
* `import cmath`
* `import math`

## python Quadratics

Python turtle example: Quadratics

```python
#!/usr/local/bin/python3

'''This module requires Python 3 Unicode Support to function'''

import math
import cmath
from turtle import *

class Quadratic(object):
    def __init__(self, a, b, c):
        self.a = a
        self.b = b
        self.c = c
        if b < 0:
            bsign = ' - '
        else:
            bsign = " + "

        if c < 0:
            csign = ' - '
        else:
            csign = " + "
        writea = "" if self.a == 1 and self.a != 0 else "-" if self.a == -1 else abs(self.a)
        writeb = "" if self.b == 1 and self.b != 0 else "-" if self.b == -1 else abs(self.b)

        if a == 0:
            apart = ""
        else:
            apart = "{}x\u00B2".format(writea)
        if b == 0:
            bpart = ""
            bsign = ' '
        else:
            bpart = "{}x".format(writeb)
        if c == 0:
            cpart = ""
            csign = ' '
        else:
            cpart = "{}".format(abs(self.c))
        self.polynomial = "{}{}{}{}{}".format(apart, bsign, bpart, csign, cpart)

    @classmethod
    def quadratic_formula(cls):
        return("\nQuadratic Formula: \n"
               " -b ± √((b^2) - 4ac)\n"
               "------------------- = x\n"
               "{}2a\n".format(" "*7))

    def roots(self):
        return (self.minusroot(), self.plusroot())

    def plusroot(self):
        return (-self.b + self.discriminant()) / (2*self.a)

    def minusroot(self):
        return (-self.b - self.discriminant()) / (2*self.a)

    def discriminant(self):
        try:
            return math.sqrt(self.b*self.b - 4*self.a*self.c)
        except ValueError:
            return cmath.sqrt(self.b*self.b - 4*self.a*self.c)

    def hasRealRoots(self):
        if isinstance(self.discriminant(), complex):
            return False
        return True

    def evaluate(self, x):
        return (x, (self.a * (x ** 2)) + (self.b * x) + (self.c))

    def shortEval(self, x):
        return self.evaluate()[1]

    def shortEvalWithRoots(self):
        return self.shortEval(self.plusroot()), self.shortEval(self.minusroot())

    def evaluateWithRoots(self):
        return (self.evaluate(self.minusroot()), self.evaluate(self.plusroot()))

    def turningPoint(self):
        tp = (sum(self.roots()) / 2)
        return (self.evaluate(tp))

def main():
    bob = Turtle()
    print(Quadratic.quadratic_formula())

    bob.penup()
    screen = bob.getscreen()
    font = ("Courier", "16", "bold")
    a, b, c = [int(x) for x in screen.textinput("Enter Constants", "Enter the constants to your quadratic in the form a, b, c").split(', ')]
    while a == 0:
        a = screen.numinput("Invalid input", "a cannot be zero. Please reenter a:")
    q = Quadratic(a, b, c)
    bob.goto(-300, 150)
    bob.write("Your quadratic is {}".format(q.polynomial), font=font)
    bob.goto(-300, 120)
    bob.write("Discriminant: {:.5g}".format(q.discriminant()), font=font)
    bob.goto(-300, 90)
    bob.write("Roots: x = {:.5g} and x = {:.5g}".format(q.minusroot(), q.plusroot()), font=font)
    bob.goto(-300, 60)
    bob.write("The roots are real", font=font) if q.hasRealRoots() else bob.write("The roots are imaginary ", font=font)
    bob.goto(-300, 0)
    bob.write("Solutions: ({:.5g}, {:.5g}) & \n\t   ({:.5g}, {:.5g})".format(q.evaluateWithRoots()[0][0], q.evaluateWithRoots()[0][1], q.evaluateWithRoots()[1][0], q.evaluateWithRoots()[1][1]), font=font)
    done()

if __name__ == "__main__":
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
