---
title: matplotlib example audio animation (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'audio animation'

Functions in program: 
* `def animate(i, line):`
* `def audio_callback(in_data, frame_count, time_info, status):`
* `def sinwave(f=44100, phi=0.0):`

Modules used in program: 
* `import matplotlib.animation as animation`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python audio animation

Python matplotlib example: audio animation

```python
from __future__ import division

import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from pysoundcard import Stream, continue_flag, complete_flag

number_of_blocks = 100
number_of_channels = 6


def sinwave(f=44100, phi=0.0):
    """A sinewave with frequency and phase offset (offset will be changed in animation)

    """
    return np.sin(np.arange(f) * 2 * np.pi / f + phi)


def audio_callback(in_data, frame_count, time_info, status):
    """PySoundcard callback.

    Notes
    -----

    This interface only works with PySoundCard 0.5.0, not with 0.6.0

    """
    out_data = np.zeros(
        (len(in_data), number_of_channels),
        dtype=in_data.dtype
    )

    # do something

    return (out_data, continue_flag)


def animate(i, line):
    """Each iteration, any of the elements in your plot

    Parameters
    ----------
    i : integer
        Counter, incrementing by 1, each animation call from the matplotlib
        :code:`FuncAnimation`.
    args : mixed
        any arguments set by :code:`fargs` in :code:`animation.FuncAnimation`

    """
    line.set_ydata(sinwave(phi=float(i) / 128))

fig, a = plt.subplots(1, 1)  # You may only use plt for subplots() and show()
line, = a.plot(sinwave())
stream = Stream(callback=audio_callback)
stream.start()

anim = animation.FuncAnimation(
    fig,
    animate,
    # Setting frames will only limit the number of frames rendered
    # to video, not in the interactive case
    frames=int(number_of_blocks),
    # We are passing the initially drawn line into our animation callback here
    fargs=(line,),
    # This controls the rate at which our animation updates
    interval=40,
    # See issue https://github.com/matplotlib/matplotlib/issues/5399
    init_func=lambda: None
)

plt.show()  # You may only use plt for subplots() and show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
