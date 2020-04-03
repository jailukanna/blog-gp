---
title: matplotlib example PlotN6 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'PlotN6'


Modules used in program: 
* `import matplotlib.pyplot as plt`

## python PlotN6

Python matplotlib example: PlotN6

```python
import matplotlib.pyplot as plt

fig = plt.figure()

ax = fig.add_subplot(1,1,1)

rectangle = plt.Rectangle((0.3, 0.75), 0.5, 0.15, color='green', alpha=0.7)
circle = plt.Circle((0.7, 0.2), 0.15, color='blue', alpha=0.5)
polygon = plt.Polygon([[0.15, 0.15], [0.35, 0.4],[0.5, 0.7], [0.2, 0.6]],
                   color='yellow', alpha=0.7)   #polygon connecting [[x1,y1],[x2,y2],[x3,y3]...]]


ax.add_patch(rectangle)
ax.add_patch(circle)
ax.add_patch(polygon)


plt.grid(True)
plt.axis('scaled')


plt.savefig('plots/shapes.png', dpi=400, bbox_inches='tight')

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
