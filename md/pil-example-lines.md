---
title: pil example lines (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'lines'

Functions in program: 
* `def save(lineses):`
* `def draw(lines, size=1024, alpha_each=0.1):`
* `def generate(number_of_lines, number_of_steps):`
* `def with_distances_sq_to_origin(lines):`
* `def generate_lines_across_unit_square(n, rnd):`
* `def generate_line_across_unit_square(rnd):`
* `def point_on_wall(t, wall_index):`

Modules used in program: 
* `import PIL.ImageDraw`
* `import PIL.Image`
* `import random`
* `import math`
* `import collections`
* `import bisect`

## python lines

Python pil example: lines

```python
"""Draw lines through a unit square. Draw the central ones.

Usage: run it, then convert all the /tmp/%04d.png files to a movie or whatever.
"""

import bisect
import collections
import math
import random

import PIL.Image
import PIL.ImageDraw


class Point(object):

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def distance_sq(self, other):
        return (self.x - other.x) ** 2 + (self.y - other.y) ** 2

    def distance(self, other):
        return math.sqrt(self.distance(other))


class Line(object):

    def __init__(self, p1, p2):
        self.p1 = p1
        self.p2 = p2

    def midpoint(self):
        mx = (self.p1.x + self.p2.x) / 2
        my = (self.p1.y + self.p2.y) / 2
        return Point(mx, my)


def point_on_wall(t, wall_index):
    """Return a point a proportion `t` along the given wall.

    As `t` moves from `0` to `1` and `wall_index` moves from `0` to `3`,
    the point will move counterclockwise from bottom-right. So wall index 0 is
    the right wall, and `t = 0` along that wall is the bottom of the wall,
    while `t = 1` is the top. Then, wall index 1 is the top wall, and `t = 0`
    is the right corner of that wall, while `t = 1` is the left. And so on.
    """

    assert 0 <= wall_index < 4

    if wall_index == 0:
        return Point(0.5, t - 0.5)
    if wall_index == 1:
        return Point(0.5 - t, 0.5)
    if wall_index == 2:
        return Point(-0.5, 0.5 - t)
    if wall_index == 3:
        return Point(t - 0.5, -0.5)


def generate_line_across_unit_square(rnd):
    """Generate a line connecting two distinct walls of the unit square."""
    (wall_index_1, wall_index_2) = rnd.sample(range(4), 2)
    return Line(point_on_wall(rnd.random(), wall_index_1),
                point_on_wall(rnd.random(), wall_index_2))


def generate_lines_across_unit_square(n, rnd):
    """Generate lots of lines connecting distinct walls of the unit square."""
    return [generate_line_across_unit_square(rnd)
            for _ in xrange(n)]


def with_distances_sq_to_origin(lines):
    """Annotate lines with their distances to the origin."""
    origin = Point(0, 0)
    return [(line, line.midpoint().distance_sq(origin))
            for line in lines]


def generate(number_of_lines, number_of_steps):
    """Generate lines, and group them by how far they are from the origin.

    Specifically, a call to `generate(m, n)` will return a list of length `n`.
    Each element of the list is itself a list, consisting of up to `m` lines;
    in particular, sublist `i` will contain all the generated lines whose
    closest distance to the center is at most `0.5 * i / n`.

    For example, if `n = 4`, then the first sublist will contain only a line
    that passes _exactly_ through the center (not likely); the second will
    contain those lines passing within 0,125 of the center; etc.

    Thus, you should expect the last sublist to contain almost `m` lines.
    """
    rnd = random.Random(0)
    lines = generate_lines_across_unit_square(number_of_lines, rnd)
    sorted_measured_lines = sorted(with_distances_sq_to_origin(lines),
                                   key=lambda (line, dist): dist)
    (sorted_lines, sorted_distances_sq) = map(list,
                                              zip(*sorted_measured_lines))
    tolerances = ((x / float(number_of_steps)) ** 2 / 2
                  for x in xrange(number_of_steps))
    cutoffs = (bisect.bisect_left(sorted_distances_sq, x)
               for x in tolerances)
    slices = (sorted_lines[:n] for n in cutoffs)
    return list(slices)


def draw(lines, size=1024, alpha_each=0.1):
    """Given a list of lines, draw them semitransparently on an image."""

    # These really counterintuitive mode declarations taken from:
    # http://stackoverflow.com/a/21768191.
    img = PIL.Image.new('RGB', (size, size))
    draw = PIL.ImageDraw.Draw(img, 'RGBA')

    color = (255, 255, 255, int(alpha_each * 255))
    def normalize(t):
        return int((t + 0.5) * size)
    def line_to_coords(line):
        return (normalize(line.p1.x), normalize(line.p1.y),
                normalize(line.p2.x), normalize(line.p2.y))
    for line in lines:
        draw.line(line_to_coords(line), fill=color)
    del draw
    return img


def save(lineses):
    """Save all the given lists of lines to consecutive images in /tmp."""
    # I realized after I wrote this that it's unnecessarily quadratic, but also
    # it takes less than a minute and I already ran it so /shrug/
    for (i, lines) in enumerate(lineses):
        print(i)
        with open('/tmp/%04d.png' % i, 'wb') as outfile:
            draw(lines).save(outfile, format='png')


if __name__ == '__main__':
    save(generate(10000, 100))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
