---
title: matplotlib example animation-example2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'animation-example2'

Functions in program: 
* `def _main():`
* `def update(frame, ax, lines):`
* `def init():`

Modules used in program: 
* `import matplotlib.animation as mplanim`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python animation-example2

Python matplotlib example: animation-example2

```python
# fast plot

import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as mplanim


def init():
    print("init")


def update(frame, ax, lines):
    x = np.arange(0, 1, 0.01)
    y = np.sin(5 * x + frame)

    lines[0].set_xdata(x)
    lines[0].set_ydata(y)


def _main():
    fig = plt.figure()
    ax = fig.add_subplot(111)
    lines = ax.plot([], [], marker='+')  # empty lines
    ax.set_xlim([0, 1])
    ax.set_ylim([-1, 1])

    fps = 60
    frames = np.arange(0, 100, 0.1)
    anim = mplanim.FuncAnimation(
        fig,
        func=update,
        frames=frames,
        init_func=init,
        interval=1000. / fps,  # msec
        fargs=[ax, lines],
        save_count=len(frames),
    )

    # anim.save('test.mp4', fps=fps)
    plt.show()


if __name__ == '__main__':
    _main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
