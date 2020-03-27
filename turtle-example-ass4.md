---
title: turtle example ass4 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'ass4'

Functions in program: 
* `def main():`
* `def loop(bodies):`
* `def update_info(step, bodies):`

Modules used in program: 
* `import math`

## python ass4

Python turtle example: ass4

```python
#!/usr/bin/env python3

import math
from turtle import *

# The gravitational constant G
G = 6.67428e-11

# Assumed scale: 100 pixels = 1AU.
AU = (149.6e6 * 1000)     # 149.6 million km, in meters.
# SCALE = 50 / AU
SCALE = 5 / AU

class Body(Turtle):
    """Subclass of Turtle representing a gravitationally-acting body.

    Extra attributes:
    mass : mass in kg
    vx, vy: x, y velocities in m/s
    px, py: x, y positions in m
    """

    name = 'Body'
    mass = None
    vx = vy = 0.0
    px = py = 0.0

    def attraction(self, other):
        """(Body): (fx, fy)

        Returns the force exerted upon this body by the other body.
        """
        # Report an error if the other object is the same as this one.
        if self is other:
            raise ValueError("Attraction of object %r to itself requested"
                             % self.name)

        # Compute the distance of the other body.
        sx, sy = self.px, self.py
        ox, oy = other.px, other.py
        dx = (ox-sx)
        dy = (oy-sy)
        d = math.sqrt(dx**2 + dy**2)

        # Report an error if the distance is zero; otherwise we'll
        # get a ZeroDivisionError exception further down.
        if d == 0:
            raise ValueError("Collision between objects %r and %r"
                             % (self.name, other.name))

        # Compute the force of attraction
        f = G * self.mass * other.mass / (d**2)

        # Compute the direction of the force.
        theta = math.atan2(dy, dx)
        fx = math.cos(theta) * f
        fy = math.sin(theta) * f
        return fx, fy

def update_info(step, bodies):
    """(int, [Body])

    Displays information about the status of the simulation.
    """
    print('Step #{}'.format(step))
    for body in bodies:
        s = '{:<8}  Pos.={:>6.2f} {:>6.2f} Vel.={:>10.3f} {:>10.3f}'.format(
            body.name, body.px/AU, body.py/AU, body.vx, body.vy)
        print(s)
    print()

def loop(bodies):
    """([Body])

    Never returns; loops through the simulation, updating the
    positions of all the provided bodies.
    """
    timestep = 24*3600  # One Day

    for body in bodies:
        body.penup()
        body.hideturtle()

    step = 1
    while True:
        update_info(step, bodies)
        step += 1

        if step > 365000: # once you've hit 1000 years break
            break

        force = {}
        for body in bodies:
            # Add up all of the forces exerted on 'body'.
            total_fx = total_fy = 0.0
            for other in bodies:
                # Don't calculate the body's attraction to itself
                if body is other:
                    continue
                fx, fy = body.attraction(other)
                total_fx += fx
                total_fy += fy

            # Record the total force exerted.
            force[body] = (total_fx, total_fy)

        # Update velocities based upon on the force.
        for body in bodies:
            fx, fy = force[body]
            body.vx += fx / body.mass * timestep
            body.vy += fy / body.mass * timestep

            # Update positions
            body.px += body.vx * timestep
            body.py += body.vy * timestep
            body.goto(body.px*SCALE, body.py*SCALE)
            body.dot(3)


def main():
    sun = Body()
    sun.name = 'Sun'
    sun.mass = 1.98892 * 10**30
    sun.pencolor('yellow')

    mercury = Body()
    mercury.name = 'Mercury'
    mercury.mass = 3.302 * 10**23
    mercury.px = -21052621110.32039
    mercury.py = -66406638083.534035
    mercury.vx = 36652.987063938395
    mercury.vy = -12289.838101110769
    mercury.pencolor('#E2E2E2')

    # Venus parameters taken from
    # http://nssdc.gsfc.nasa.gov/planetary/factsheet/venusfact.html
    venus = Body()
    venus.name = 'Venus'
    venus.mass = 4.8685 * 10**24
    venus.px = -107505550269.5123
    venus.py = -3366520720.591562
    venus.vx = 889.1598046362434
    venus.vy = -35159.2077412429
    venus.pencolor('red')

    earth = Body()
    earth.name = 'Earth'
    earth.mass = 5.9742 * 10**24
    earth.px = -25210928638.52298
    earth.py = 144927919571.2076
    earth.vx = -29839.83333368269
    earth.vy = -5207.633918704476
    earth.pencolor('blue')

    mars = Body()
    mars.name = 'Mars'
    mars.mass = 6.390 * 10**23
    mars.px = 207995054990.8331
    mars.py = -3143009561.106971
    mars.vx = 1295.0035328516021
    mars.vy = 26294.42067068712
    mars.pencolor('#c1440e')

    jupiter = Body()
    jupiter.name = 'Jupiter'
    jupiter.mass = 1.898 * 10**27
    jupiter.px = 598909159497.3032
    jupiter.py = 439122593153.051
    jupiter.vx = -7901.937610713569
    jupiter.vy = 11163.17695450082
    jupiter.pencolor('#d8ca9d')

    saturn = Body()
    saturn.name = 'Saturn'
    saturn.mass = 5.683 * 10**26
    saturn.px = 958706336820.0247
    saturn.py = 982565210912.1954
    saturn.vx = -7428.885683466338
    saturn.vy = 6738.8142377173735
    saturn.pencolor('#e3e0c0')

    uranus = Body()
    uranus.name = 'Uranus'
    uranus.mass = 8.681 * 10**25
    uranus.px = 2158774703477.1318
    uranus.py = -2054825231595.053
    uranus.vx = 4637.648411798584
    uranus.vy = 4627.192877193527
    uranus.pencolor('#E2E2E2')

    neptune = Body()
    neptune.name = 'Neptune'
    neptune.mass = 1.024 * 10**26
    neptune.px = 2514853420151.505
    neptune.py = -3738847412364.252
    neptune.vx = 4465.799984073191
    neptune.vy = 3075.681163952201
    neptune.pencolor('#274687s')

    pluto = Body()
    pluto.name = 'Pluto'
    pluto.mass = 1.303 * 10**23
    pluto.px = -1477558339934.327
    pluto.py = -4182460438550.376
    pluto.vx = 5261.92568969292
    pluto.vy = -2648.919644838698
    pluto.pencolor('#f6dddb')

    ceres = Body()
    ceres.name = 'Ceres'
    ceres.mass = 8.958 * 10**20
    ceres.px = -2.800 * AU
    ceres.px = -355673458269.5031
    ceres.py = 119794528995.9168
    ceres.vx = -6242.632308995013
    ceres.vy = -18316.79465274023
    ceres.pencolor('#000')

    vesta = Body()
    vesta.name = 'Vesta'
    vesta.mass = 2.589 * 10**20
    vesta.px = -2.500 * AU
    vesta.px = -203211917524.47922
    vesta.py = -249743051171.24612
    vesta.vx = 16630.606117409377
    vesta.vy = -12859.82826836888
    vesta.pencolor('#000')

    pallas = Body()
    pallas.name = 'Pallas'
    pallas.mass = 2.108 * 10**20
    pallas.px = -124954059801.9996
    pallas.py = 247703297527.03662
    pallas.vx = -20344.831154861167
    pallas.vy = -7093.994081177277
    pallas.pencolor('#000')

    hygiea = Body()
    hygiea.name = 'Hygiea'
    hygiea.mass = 8.670 * 10**19
    hygiea.px = -355611115169.47906
    hygiea.py = -218277280811.7634
    hygiea.vx = 10550.84895383623
    hygiea.vy = -15512.51245570737
    hygiea.pencolor('#000')

    interamnia = Body()
    interamnia.name = 'Interamnia'
    interamnia.mass = 3.900 * 10**19
    interamnia.px = -203581333070.74948
    interamnia.py = -443895089712.2372
    interamnia.vx = 14207.70531575802
    interamnia.vy = -5201.659902083895
    interamnia.pencolor('#000')

    # loop([sun, mercury, venus, earth, mars, jupiter, ceres, vesta, pallas, hygiea, interamnia])
    loop([sun, jupiter, saturn, uranus, neptune, pluto])

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
