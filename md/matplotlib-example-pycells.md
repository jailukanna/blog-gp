---
title: matplotlib example pycells (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pycells'

Functions in program: 
* `def dotest(count, size, times):`

Modules used in program: 
* `import matplotlib.pyplot`
* `import matplotlib`

## python pycells

Python matplotlib example: pycells

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-


from random import randint
from time import clock
import matplotlib
import matplotlib.pyplot


class RuleSet(object):
    """Contains a list of rules and handles application of them on a
    position
    """

    def __init__(self, isTotalistic, lives, borne):
        """

        Arguments:
        - `isTotalistic`: totalistic ruleset or not (Boolean)
        - `lives`: list of neighborhoods (nested lists) or number of
          neighbors leading to life
        - `borne`: list of neighborhoods (nested lists) or number of
          neighbors leading to life
        """
        self.isTotalistic = isTotalistic
        self.lives = lives
        self.borne = borne

    def apply(self, neighborhood):
        if self.isTotalistic:
            if (0, 0) in neighborhood:
                return len(neighborhood) - 1 in self.lives
            else:
                return len(neighborhood) in self.borne
        else:
            if (0, 0) in neighborhood:
                return neighborhood in self.lives
            else:
                return neighborhood in self.borne


class Automaton(object):
    """Cellular automaton, handles the actual tracking and evolution
    of living stones. Therefore - needs speed optimization, possibly
    by writing a c extension
    """

    def __init__(self, ruleSet, living=[]):
        """The initial living stones must be supplied

        Arguments:
        - `ruleSet`: ruleset object
        - `living`: list of position tuples
        """
        self.ruleSet = ruleSet
        self.living = living
        self.lastStep = []

    def __str__(self):
        min_x = min([x[0] for x in self.living])
        max_x = max([x[0] for x in self.living])
        min_y = min([x[1] for x in self.living])
        max_y = max([x[1] for x in self.living])
        retstr = ""
        for i in range(min_x, max_x + 1):
            for j in range(min_y, max_y + 1):
                if (i, j) in self.living:
                    if (i, j) != (0, 0):
                        retstr += "[]"
                    else:
                        retstr += "##"
                else:
                    if (i, j) != (0, 0):
                        retstr += "  "
                    else:
                        retstr += "--"
            retstr += "\n"
        return retstr

    def apply(self, pos):
        i, j = pos
        neighborhood = [
            (x, y) for x in [-1, 0, 1] for y in [-1, 0, 1]
            if (i + x, j + y) in self.lastStep]
        return self.ruleSet.apply(neighborhood)

    def step(self):
        """Steps the automaton
        """
        self.lastStep = self.living

        """From the currently living positions, gets the relevant
        positions to apply the ruleset on:

        the positions which are within neighborhood radius from living
        stones - find by checking positions around each of the living
        stones, marking checked positions
        """
        checked = []
        for (i, j) in self.lastStep:
            for pos in [
                (i + x, j + y) for x in [-1, 0, 1] for y in [-1, 0, 1]]:
                if pos not in checked:
                    if self.apply(pos):
                        checked.append(pos)
        self.living = checked


def dotest(count, size, times):
    lives = [2, 3]
    borne = [3]
    ruleSet = RuleSet(isTotalistic=True, lives=lives, borne=borne)
    living = [
        (randint(-size, size), randint(-size, size))
        for x in range(count) for y in range(count)]
    automaton = Automaton(ruleSet=ruleSet, living=living)
    timelist = []
    for i in range(times):
        t = clock()
        automaton.step()
        t = clock() - t
        timelist.append(t)
    return timelist


if __name__ == '__main__':
    table = {}
    times = 2000
    for count in [20]:
        table.update({count: dotest(count, 20, times)})

    matplotlib.pyplot.hold(True)
    for key in sorted(table.iterkeys()):
        label = str(key) + " aktiva celler"
        matplotlib.pyplot.plot(range(times),
                               table[key],
                               label=label)
    matplotlib.pyplot.legend()
    matplotlib.pyplot.xlabel("Iteration [enhetslos]")
    matplotlib.pyplot.ylabel("Berakningstid [s]")
    matplotlib.pyplot.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
