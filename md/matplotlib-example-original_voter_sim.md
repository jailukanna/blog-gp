---
title: matplotlib example original voter sim (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'original voter sim'

Functions in program: 
* `def update():`
* `def check_consensus():`
* `def observe():`
* `def initialize():`

Modules used in program: 
* `import numpy as np`
* `import random as rd`
* `import networkx as nx`
* `import matplotlib`

## python original voter sim

Python matplotlib example: original voter sim

```python
import matplotlib
matplotlib.use('TkAgg')
from pylab import *
import networkx as nx
import random as rd
import numpy as np



def initialize():
    global g, steps
    steps = 0
    g = nx.karate_club_graph()
    g.pos = nx.spring_layout(g)
    for i in g.nodes:
        g.nodes[i]['state'] = 1 if random() < .5 else 0

def observe():
    global g, steps
    cla()
    nx.draw(g, vmin = 0, vmax = 1,
            node_color = [g.nodes[i]['state'] for i in g.nodes],
            pos = g.pos)

def check_consensus():
    global g, steps
    consensus = True
    vote = g.nodes[0]['state']
    for i in g.nodes:
        if g.nodes[i]['state'] != vote:
            consensus = False
    if consensus:
        return steps
    else:
        return False


def update():
    global g, steps
    listener = rd.choice(list(g.nodes))
    speaker = rd.choice(list(g.neighbors(listener)))
    g.nodes[listener]['state'] = g.nodes[speaker]['state']
    steps += 1

steps_reach_consensus = []
for i in range(1000):
    initialize()
    check_consensus()
    while check_consensus() == False:
        update()
    steps_reach_consensus.append(check_consensus())

print(np.mean(steps_reach_consensus))
# >>> 611.034

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
