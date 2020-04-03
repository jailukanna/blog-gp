---
title: matplotlib example distribution gif (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'distribution gif'

Functions in program: 
* `def frame(epoch):`

Modules used in program: 
* `import imageio`
* `import matplotlib.pyplot as plt`
* `import random`
* `import seaborn as sns`

## python distribution gif

Python matplotlib example: distribution gif

```python
import seaborn as sns
import random
import matplotlib.pyplot as plt
import imageio


def frame(epoch):
    a = []
    for _ in range(10000):
        a.append(random.random())

    a = np.array(a)
    print(a)
    plt.figure() 
    sns_hist = sns.distplot(a)
    plt.title(str(epoch))

    ax = plt.gca()

    ax.figure.canvas .draw()       # draw the canvas, cache the renderer
    image = np.frombuffer(ax.figure.canvas.tostring_rgb(), dtype='uint8')
    image = image.reshape(ax.figure.canvas.get_width_height()[::-1] + (3,))
    return image


images = []
for i in range(10):
    images.append(frame(i))

kwargs_write = {'fps':1.0, 'quantizer':'nq'}
imageio.mimsave('./powers.gif', images, fps=1)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
