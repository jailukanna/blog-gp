---
title: matplotlib example plot bullet graph sales performance against benchmark (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot bullet graph sales performance against benchmark'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python plot bullet graph sales performance against benchmark

Python matplotlib example: plot bullet graph sales performance against benchmark

```python
"""
B-055 - Building Bullet Graphs and Waterfall Charts with Bokeh
https://pbpython.com/bokeh-bullet-waterfall.html
"""
# enable Bokeh’s output to display
import pandas as pd
import numpy as np
from bokeh.io import show, output_notebook
from bokeh.palettes import PuBu4
from bokeh.plotting import figure
from bokeh.models import Label

output_notebook()
# BokehJS 0.13.0 successfully loaded.
# Connects to a saved book (looks in all Excel instances)
wb1 = xw.Book('C:/Users/Joseph Yu/Desktop/Bullet-Graphs-and-Waterfall-Charts-with-Bokeh.xlsx')
sheet = wb1.sheets[0]
sheet.range('A1').expand('table').value

data = sheet.range('A1').expand('table').value
# Load in the data
data= [['John Smith', 105.0, 120.0], ['Jane Jones', 99.0, 110.0], ['Fred Flintstone', 109.0, 125.0], ['Barney Rubble', 135.0, 123.0], ['Mr T', 45.0, 105.0]]
limits = [0, 20, 60, 100, 160]
labels = ["Poor", "OK", "Good", "Excellent"]
cats = [x[0] for x in data]
# Load in the data
data= [("John Smith", 105, 120),
       ("Jane Jones", 99, 110),
       ("Fred Flintstone", 109, 125),
       ("Barney Rubble", 135, 123),
       ("Mr T", 45, 105)]

limits = [0, 20, 60, 100, 160]
labels = ["Poor", "OK", "Good", "Excellent"]
cats = [x[0] for x in data]
create the Bokeh figure and set a couple of options
# Create the base figure
p=figure(title="Sales Rep Performance", plot_height=350, plot_width=800, y_range=cats)
p.x_range.range_padding = 0
p.grid.grid_line_color = None
p.xaxis[0].ticker.num_minor_ticks = 0
# Here's the format of the data we need
print(list(zip(limits[:-1], limits[1:], PuBu4[::-1])))
[(0, 20, '#f1eef6'), (20, 60, '#bdc9e1'), (60, 100, '#74a9cf'), (100, 160, '#0570b0')]
for left, right, color in zip(limits[:-1], limits[1:], PuBu4[::-1]):
    p.hbar(y=cats, left=left, right=right, height=0.8, color=color)
show(p)
# Now add the black bars for the actual performance
perf = [x[1] for x in data]
p.hbar(y=cats, left=0, right=perf, height=0.3, color="black")
show(p)
# Add the segment for the target
comp = [x[2]for x in data]
p.segment(x0=comp, y0=[(x, -0.5) for x in cats], x1=comp, 
          y1=[(x, 0.5) for x in cats], color="white", line_width=2)
show(p)
# Add the labels
for start, label in zip(limits[:-1], labels):
    p.add_layout(Label(x=start, y=0, text=label, text_font_size="10pt",
                       text_color='black', y_offset=5, x_offset=15))
show(p)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
