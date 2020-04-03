---
title: matplotlib example edge voter sim (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'edge voter sim'

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

## python edge voter sim

Python matplotlib example: edge voter sim

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
    edge = rd.choice(list(g.edges))
    speaker = edge[0]
    listener = edge[1]
    g.nodes[listener]['state'] = g.nodes[speaker]['state']
    steps += 1

steps_reach_consensus = []
for i in range(1):
    initialize()
    check_consensus()
    while check_consensus() == False:
        update()
    steps_reach_consensus.append(check_consensus())

print(np.mean(steps_reach_consensus))
# Not returning

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
