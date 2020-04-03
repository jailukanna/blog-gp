---
title: matplotlib example return image and plot with flask (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'return image and plot with flask'

Functions in program: 
* `def generate_plot():`
* `def generate_image():`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import cStringIO as StringIO`

## python return image and plot with flask

Python matplotlib example: return image and plot with flask

```python
"""
Demonstration of how to return an image generated with numpy and a plot
generated with matplotlib using the Flask web server.

Requirements: numpy, flask, scikit-image, matplotlib.
"""

import cStringIO as StringIO
from flask import Flask, send_file
import numpy as np
from skimage.io import imsave
import matplotlib.pyplot as plt

app = Flask(__name__)
app.debug = True


@app.route('/')
def generate_image():
    """
    Return a generated image as a png by
    saving it into a StringIO and using send_file.
    """
    num_tiles = 20
    tile_size = 30
    arr = np.random.randint(0, 255, (num_tiles, num_tiles, 3))
    arr = arr.repeat(tile_size, axis=0).repeat(tile_size, axis=1)

    # We make sure to use the PIL plugin here because not all skimage.io plugins
    # support writing to a file object.
    strIO = StringIO.StringIO()
    imsave(strIO, arr, plugin='pil', format_str='png')
    strIO.seek(0)
    return send_file(strIO, mimetype='image/png')


@app.route('/plot')
def generate_plot():
    """
    Return a matplotlib plot as a png by
    saving it into a StringIO and using send_file.
    """
    def using_matplotlib():
        fig = plt.figure(figsize=(6, 6), dpi=300)
        ax = fig.add_subplot(111)
        x = np.random.randn(500)
        y = np.random.randn(500)
        ax.plot(x, y, '.', color='r', markersize=10, alpha=0.2)
        ax.set_title('Behold')

        strIO = StringIO.StringIO()
        plt.savefig(strIO, dpi=fig.dpi)
        strIO.seek(0)
        return strIO

    strIO = using_matplotlib()
    return send_file(strIO, mimetype='image/png')


if __name__ == '__main__':
    app.run()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
