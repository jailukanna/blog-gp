---
title: matplotlib example animate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'animate'

Functions in program: 
* `def animate(i):  # i is the frame number`
* `def init():`

Modules used in program: 
* `import numpy as np`
* `import matplotlib`

## python animate

Python matplotlib example: animate

```python
import matplotlib
import numpy as np
from matplotlib import pyplot as plt
from matplotlib import animation

# first set up the figure and axis for the animation
fig, ax = plt.subplots()
line, = ax.plot([], [])
ax.set_xlim(0, 2)
ax.set_ylim(-2, 2)

# initialization function: plot the background of each frame
def init():
    line.set_data([], [])
    return line,

# set up animation function
def animate(i):  # i is the frame number
    x = np.linspace(0, 2, 1000)
    y = np.sin(2 * np.pi * (x - 0.01 * i))  # plot 
    line.set_data(x, y)
    return line,

# call the animator:  blit=True means only re-draw the parts that have changed.
anim = animation.FuncAnimation(fig, animate, init_func=init,
                               frames=200, interval=20, blit=True)
#plt.show()  # uncomment to watch
anim.save('wave.mp4', fps=30)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
