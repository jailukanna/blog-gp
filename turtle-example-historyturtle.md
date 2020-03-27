---
title: turtle example historyturtle (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'historyturtle'


## python historyturtle

Python turtle example: historyturtle

```python
from turtle import Turtle

"""
Python Turtle with logging and replay capabilities

Author: João S. O. Bueno <gwidion@gmail.com>
License: LGPL 3.0+

This implements HistoryTurtle - a subclass of
Python's turtle.Turtle wich features a simple
command history and   new `replay` and `log`
methods -
`turtle.replay(<start>, <end>)` will reissue
the commands logged in those positions of the history,
but with the current Pen settings.

The optional kwarg `position_independent` can be passed
as `True` to `replay` so that the turtle's position
and heading are not restored to the ones recorded
along with the command.

https://gist.github.com/jsbueno/cb413e985747392c460f39cc138436bc

"""


class Command:
    __slots__ = ( "method_name", "args", "kwargs", "pos", "heading")

    def __init__(self, method_name, args=(),
                 kwargs=None, pos=None, heading=None):
        self.method_name = method_name
        self.args = args
        self.kwargs = kwargs or {}
        self.pos = pos
        self.heading = heading

    def __repr__(self):
        return "<{0}> - {1}".format(self.pos, self.method_name)


class Instrumented:
    def __init__(self, turtle, method):
        self.turtle = turtle
        self.method = method

    def __call__(self, *args, **kwargs):
        command = Command(self.method.__name__, args, kwargs,
                          self.pos(), self.heading())
        result = self.method(*args, **kwargs)
        if (self.method.__name__ not in self.turtle.methods_to_skip or
                not kwargs.pop('skip_self', True)):
            self.turtle.history.append(command)
        return result

    def pos(self):
        return self.turtle._execute(Command('pos'), True)

    def heading(self):
        return self.turtle._execute(Command('heading'), True)


class HistoryTurtle(Turtle):

    methods_to_skip = ('replay', '_execute', 'log')

    def __init__(self, *args, **kw):
        self._inited = False
        super().__init__(*args, **kw)
        self.history = []
        self._replaying = [False]
        self._inited = True

    def __getattribute__(self, attr):
        result = super().__getattribute__(attr)
        if (not callable(result) or
                attr.startswith('_') or
                not self._inited or
                self._replaying[-1]):
            return result
        return Instrumented(self, result)

    def replay(self, start, end=None, *,
               position_independent=False,
               skip_self=True,
               restore=True
               ):
        results = []

        self._replaying.append(True)
        if restore:
            pos, heading, state = self.pos(), self.heading(), self.pen()['pendown']
        for command in self.history[start:end]:
            results.append(
                self._execute(command, position_independent))
        if restore:
            self.setpos(pos)
            self.setheading(heading)
            (self.pendown() if state else self.penup())
        self._replaying.pop()
        return results

    def log(self, start=0, end=None):
        for i, command in enumerate(self.history[start:end], start):
            print(i, command)

    def _execute(self, command, position_independent):
        """ Execute a method without triggering the log
        """
        # Warning: not thread-safe:
        self._replaying.append(True)
        method = getattr(self, command.method_name)
        if not position_independent:
            state = self.pen()['pendown']
            self.setpos(command.pos)
            self.setheading(command.heading)
            (self.pendown() if state else self.penup())
        result = method(*command.args, **command.kwargs)
        self._replaying.pop()
        return result


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
