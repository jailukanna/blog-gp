---
title: PIL example pyvisgraph example (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'pyvisgraph example'


Modules used in program: 
* `import pyvisgraph as vg`
* `import PIL.ImageDraw`
* `import PIL`
* `import numpy as np`
* `import matplotlib.ticker as ticker`
* `import matplotlib.pyplot as plt`

## python pyvisgraph example

Python PIL example: pyvisgraph example

```python
import matplotlib.pyplot as plt
import matplotlib.ticker as ticker
import numpy as np
import PIL
import PIL.ImageDraw
import pyvisgraph as vg

polys = [[vg.Point(3.0,2.0), vg.Point(6.0,2.0), vg.Point(6.0,7.0), vg.Point(3.0,7.0)]]
g = vg.VisGraph()
g.build(polys)
s  = vg.Point(1.0,5.0)
e = vg.Point(8.0,5.0)
path = g.shortest_path(s, e)
print(path)

im = PIL.Image.new('RGB', (10,10), (255,255,255))
draw = PIL.ImageDraw.Draw(im, 'RGBA')
points = [(p.x,p.y) for p in polys[0]]

p1 = path[0]
for p2 in path[1:]:
    draw.line((p1.x,p1.y,p2.x,p2.y),(255,0,0))
    p1 = p2

draw.polygon(points, fill = None, outline = (0,0,0,160))

plt.text(s.x, s.y, 'S', horizontalalignment='center', verticalalignment='center')
plt.text(e.x, e.y, 'E', horizontalalignment='center', verticalalignment='center')

plt.imshow(im, origin='lower')

ax = plt.gca()
ax.set_xticks(np.arange(0, im.size[0], 1));
ax.set_yticks(np.arange(0, im.size[1], 1));

# Labels for major ticks
ax.set_xticklabels(np.arange(0, im.size[0], 1));
ax.set_yticklabels(np.arange(0, im.size[1], 1));

# Minor ticks
ax.set_xticks(np.arange(-.5, im.size[0], 1), minor=True);
ax.set_yticks(np.arange(-.5, im.size[1], 1), minor=True);

# Gridlines based on minor ticks
ax.grid(which='minor', color='gray', linestyle='-', linewidth=1.5)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
