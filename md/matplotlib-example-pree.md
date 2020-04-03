---
title: matplotlib example pree (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pree'

Functions in program: 
* `def update():`
* `def pref_select(nds):`
* `def observe():`
* `def initialize():`
* `def update():`
* `def observe():`
* `def initialize():`
* `def update():`
* `def observe():`
* `def initialize():`

Modules used in program: 
* `import networkx as nx`
* `import matplotlib`
* `import random as rd`
* `import networkx as nx`
* `import matplotlib`
* `import random as rd`
* `import networkx as nx`
* `import matplotlib`

## python pree

Python matplotlib example: pree

```python
# Revise the small­world network formation model
# above so that the network is initially a two­dimensional grid in which each node is
# connected to its four neighbors. (north, south, east, and west; except for those on the
# boundaries of the space). Then run the simulations, and see how random edge rewiring
# changes the topology of the network.

# In[5]:


#original code 
import matplotlib
#matplotlib.use('TkAgg')
from pylab import *
import networkx as nx
import random as rd

n = 30 # number of nodes
k = 4 # number of neighbors of each node

def initialize():
    global g
    g = nx.Graph()
    for i in range(n):
        for j in range(1, k // 2 + 1):
            g.add_edge(i, (i + j) % n)
            g.add_edge(i, (i - j) % n)
    g.pos = nx.spring_layout(g)
    g.count = 0

def observe():
    global g
    cla()
    nx.draw(g, pos = g.pos)

def update():
    global g
    g.count += 1
    if g.count % 20 == 0: # rewiring once in every 20 steps
        nds = list(g.nodes)
        i = rd.choice(nds)
        if g.degree[i] > 0:
            g.remove_edge(i, rd.choice(list(g.neighbors(i))))
            nds.remove(i)
            for j in g.neighbors(i):
                nds.remove(j)
            g.add_edge(i, rd.choice(nds))

    # simulation of node movement
    g.pos = nx.spring_layout(g, pos = g.pos, iterations = 5)

#import pycxsimulator
#pycxsimulator.GUI().start(func=[initialize, observe, update])


get_ipython().run_line_magic('matplotlib', 'inline')
initialize()
observe()
plt.figure

ntimes = 50
for i in range(ntimes):
    update()
    plt.figure
    observe()


# In[15]:


#Adapted code 
import matplotlib
#matplotlib.use('TkAgg')
from pylab import *
import networkx as nx
import random as rd

n = 30 # number of nodes
k = 4 # number of neighbors of each node
p = 20 # rewiring once in every 20 steps

#this is where I need to make the change
#initialise that each node is connected
#to its 4 nearest neighbours
def initialize():
    global g
    g = nx.Graph()
    for i in range(n):
        #trying to do nearest neighbours with this...
        for j in [-1, 0, 1]:
            g.add_edge(i, (i + j) % n)
            g.add_edge(i, (i - j) % n)
    g.pos = nx.spring_layout(g)
    g.count = 0


def observe():
    global g
    cla()
    nx.draw(g, pos = g.pos)

    
def update():
    global g
    g.count += 1
    if g.count % p == 0: # rewiring once in every p steps
        nds = list(g.nodes)
        i = rd.choice(nds)
        if g.degree[i] > 0:
            g.remove_edge(i, rd.choice(list(g.neighbors(i))))
            nds.remove(i)
            for j in g.neighbors(i):
                nds.remove(j)
            g.add_edge(i, rd.choice(nds))

    # simulation of node movement
    g.pos = nx.spring_layout(g, pos = g.pos, iterations = 5)

#import pycxsimulator
#pycxsimulator.GUI().start(func=[initialize, observe, update])


get_ipython().run_line_magic('matplotlib', 'inline')
initialize()
observe()
plt.figure

ntimes = 50
for i in range(ntimes):
    update()
    plt.figure
    observe()


# In[16]:


#Simulations 
#random edge rewiring every 50 steps 
p = 50 
get_ipython().run_line_magic('matplotlib', 'inline')
initialize()
observe()
plt.figure


ntimes = 50
for i in range(ntimes):
    update()
    plt.figure
    observe()


# In[17]:


#Simulations 
#random edge rewiring every 5 steps 
p = 5 
get_ipython().run_line_magic('matplotlib', 'inline')
initialize()
observe()
plt.figure


ntimes = 50
for i in range(ntimes):
    update()
    plt.figure
    observe()


# Simulate the Barabási­Albert network growth
# model with m  = 1 , m  = 3 , and m  = 5 , and see how the growth process may be affected by
# the variation of this parameter.
# 
# import networkx as nx 
# G= nx.barabasi_albert_graph(50,40) 
# nx.draw(G, with_labels=True) 

# In[7]:


#code
#https://github.com/cscheffler/sayama-networkx-2/blob/master/chapter-16/16.12.py

import matplotlib
#matplotlib.use('TkAgg')
from pylab import *
import networkx as nx

m0 = 5 # number of nodes in initial condition
m = 2 # number of edges per new node

def initialize():
    global g
    g = nx.complete_graph(m0)
    g.pos = nx.spring_layout(g)
    g.count = 0

def observe():
    global g
    cla()
    nx.draw(g, pos = g.pos)

def pref_select(nds):
    global g
    r = uniform(0, sum(g.degree(i) for i in nds))
    x=0
    for i in nds:
        x += g.degree[i]
        if r <= x:
            return i

def update():
    global g
    g.count += 1
    if g.count % 20 == 0: # network growth once in every 20 steps
        nds = list(g.nodes)
        newcomer = max(nds) + 1
        for i in range(m):
            j = pref_select(nds)
            g.add_edge(newcomer, j)
            nds.remove(j)
        g.pos[newcomer] = (0, 0)

    # simulation of node movement
    g.pos = nx.spring_layout(g, pos = g.pos, iterations = 5)

#import pycxsimulator
#pycxsimulator.GUI().start(func=[initialize, observe, update])
get_ipython().run_line_magic('matplotlib', 'inline')
initialize()
observe()
plt.figure

ntimes = 100
for i in range(ntimes):
    update()
    plt.figure
    observe()


# In[8]:


m = 1 # number of edges per new node
ntimes = 100
for i in range(ntimes):
    update()
    plt.figure
    observe()


# In[9]:


m = 3 # number of edges per new node
ntimes = 100
for i in range(ntimes):
    update()
    plt.figure
    observe()


# In[10]:


m = 5 # number of edges per new node
ntimes = 100
for i in range(ntimes):
    update()
    plt.figure
    observe()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
