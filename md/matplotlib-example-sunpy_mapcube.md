---
title: matplotlib example sunpy mapcube (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sunpy mapcube'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import sunpy.map`
* `import matplotlib`
* `import glob`

## python sunpy mapcube

Python matplotlib example: sunpy mapcube

```python
import glob

import matplotlib
matplotlib.use("Agg")

import sunpy.map

import matplotlib.pyplot as plt
from matplotlib import animation

filenames = glob.glob("/home/stuart/sunpy/data/*2015*magnetogram.fits")
filenames.sort()

savdir = "/home/stuart/"

fig = plt.figure("An SDO movie")
mc = sunpy.map.Map(filenames, cube=True)

ani = mc.plot()

Writer = animation.writers['ffmpeg']
writer = Writer(fps=15, metadata=dict(artist='Me'), bitrate=1800)

ani.save(savdir+'304.mp4', writer=writer)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
