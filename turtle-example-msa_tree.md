---
title: turtle example msa tree (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'msa tree'


## python msa tree

Python turtle example: msa tree

```python
from turtle import *

class Node:
    def __init__(self, pos, layer):
        self.pos = pos
        self.layer = layer
    def __eq__(self, other):
        return self.pos == other.pos
    def __str__(self):
        return f"({self.pos[0]}, {self.pos[1]})@{self.layer}"

ANGLE = 45
LENGTH = 10
START = Node([0, -250], 0)
p=lambda:list(map(lambda i: round(i, 2), pen.pos()))

pen = Pen()
nodes = [START]
used = []

pen.speed("fastest")
pen.right(180 + ANGLE / 2)

while 1:
    node = nodes[0]
    if used.count(node) < 2:
        # print("Node confirmed unused", node)
        for angle in [ANGLE, -ANGLE]:
            pen.up()
            pen.goto(*node.pos)
            pen.down()
            pen.right(angle)
            pen.forward(LENGTH)
            pen.right(2 * angle)
            k=Node(p(), node.layer + 1)
            nodes.append(k)
            used.append(k)
        nodes.remove(node)
        nodes.sort(key=lambda i:i.layer)
    else:
        # print("Node confirmed used", node, used.count(node))
        nodes.remove(node)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
