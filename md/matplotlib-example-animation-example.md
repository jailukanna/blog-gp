---
title: matplotlib example animation-example (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'animation-example'

Functions in program: 
* `def _main():`
* `def update(frame, ax):`
* `def init():`

Modules used in program: 
* `import matplotlib.animation as mplanim`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python animation-example

Python matplotlib example: animation-example

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as mplanim


def init():
    print("init")


def update(frame, ax):
    print('update', frame)
    ax.clear()

    x = np.arange(0, 10, 0.01)
    y = np.sin(x + frame)
    ax.plot(x, y)


def _main():
    fig = plt.figure()
    ax = fig.add_subplot(111)

    fps = 30
    frames = np.arange(0, 10, 0.1)
    anim = mplanim.FuncAnimation(
        fig,
        func=update,
        frames=frames,
        init_func=init,
        interval=1000. / fps,  # msec
        fargs=[ax],
        save_count=len(frames),
    )

    #anim.save('test.mp4', fps=fps)
    plt.show()


if __name__ == '__main__':
    _main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
