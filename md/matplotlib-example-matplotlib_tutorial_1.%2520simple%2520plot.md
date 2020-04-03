---
title: matplotlib example matplotlib tutorial 1.%2520simple%2520plot (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib tutorial 1.%2520simple%2520plot'

Functions in program: 
* `def plot(mode):`
* `def f(t):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python matplotlib tutorial 1.%2520simple%2520plot

Python matplotlib example: matplotlib tutorial 1.%2520simple%2520plot

```python
import numpy as np
import matplotlib.pyplot as plt


def f(t):
    return np.exp(-t) * np.cos(2 * np.pi * t)

def plot(mode):
    if mode is 1:

        plt.plot([1 , 2, 3 , 4])
        plt.ylabel('some numbers')
        plt.show()

    elif mode is 2:

        plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
        plt.show()

    elif mode is 3:

        plt.plot([1, 2, 3, 4], [1, 4, 9, 16], 'ro')
        plt.axis([0, 6, 0, 20])
        plt.show()

    elif mode is 4:

        # evenly sampled time at 200ms intervals
        t = np.arange(0., 5., 0.2)

        # red dashes, blue squares and green triangles
        plt.plot(t, t, 'r--', t, t ** 2, 'bs', t, t ** 3, 'g^')
        plt.show()

    elif mode is 5:

        t = np.arange(0., 5., 0.2)

        plt.plot(t, t ** 2)
        plt.show()

        plt.plot(t, t ** 2, 'r', linewidth = 2.0, ls = '--', marker = '+')
        plt.show()

    elif mode is 6:

        t = np.arange(0., 5., 0.2)

        line, = plt.plot(t, t ** 2, '-')
        line.set_antialiased(False)  # turn off antialising
        line.set_marker('+')
        plt.show()

    elif mode is 7:

        t = np.arange(0., 5., 0.2)
        lines = plt.plot(t, t ** 2 , t, t ** 3)
        plt.show()
        # use keyword args
        plt.setp(lines, color='r', linewidth=2.0)
        plt.show()
        # or MATLAB style string value pairs
        plt.setp(lines, 'color', 'r', 'linewidth', 2.0)
        plt.show()

    elif mode is 8:

        t1 = np.arange(0.0, 5.0, 0.1)
        t2 = np.arange(0.0, 5.0, 0.02)

        plt.figure(1)
        plt.subplot(211)
        plt.plot(t1, f(t1), 'bo', t2, f(t2), 'k')

        plt.subplot(212)
        plt.plot(t2, np.cos(2 * np.pi * t2), 'r--')
        plt.show()

    elif mode is 9:

        plt.figure(1)  # the first figure
        plt.subplot(211)  # the first subplot in the first figure
        plt.plot([1, 2, 3])
        plt.subplot(212)  # the second subplot in the first figure
        plt.plot([4, 5, 6])

        plt.figure(2)  # a second figure
        plt.plot([4, 5, 6])  # creates a subplot(111) by default

        plt.show()

        plt.figure(1)  # figure 1 current; subplot(212) still current
        plt.subplot(211)  # make subplot(211) in figure1 current
        plt.title('Easy as 1, 2, 3')  # subplot 211 title

        plt.show()

    elif mode is 10:

        # Fixing random state for reproducibility
        np.random.seed(19680801)

        mu, sigma = 100, 15
        x = mu + sigma * np.random.randn(10000)

        # the histogram of the data
        n, bins, patches = plt.hist(x, 50, normed=1, facecolor='g', alpha=0.5)

        plt.xlabel('Smarts')
        plt.ylabel('Probability')
        plt.title('Histogram of IQ')
        plt.text(60, .025, r'$\mu=100,\ \sigma=15$')
        plt.axis([40, 160, 0, 0.03])
        plt.grid(True)
        plt.show()

    elif mode is 11:

        ax = plt.subplot(111)

        t = np.arange(0.0, 5.0, 0.01)
        s = np.cos(2 * np.pi * t)
        line, = plt.plot(t, s, lw=2)

        plt.annotate('local max', xy=(2, 1), xytext=(3, 1.5),
                     arrowprops=dict(facecolor='black', shrink=0.05),
                     )

        plt.ylim(-2, 2)
        plt.show()

plot(11)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
