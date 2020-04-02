---
title: pil example Render%2520PyPlot%2520as%2520PNG (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Render%2520PyPlot%2520as%2520PNG'


Modules used in program: 
* `import pandas`

## python Render%2520PyPlot%2520as%2520PNG

Python pil example: Render%2520PyPlot%2520as%2520PNG

```python
"""How to render a PyPlot graph as a PNG file"""

from matplotlib import pyplot
import pandas

# First, generate some PyPlot image
data = pandas.DataFrame({'a': [1,2,8], 'b': [4,7,6]})
my_plot = data.plot()
my_plot.set_title("My plot title")
my_plot.set_ylabel('Foo Label')
my_plot.set_xlabel('Bar Label  ')

# This shows the current plot on the display
pyplot.show()

# The following converts the current plot to an rendered image
canvas = pyplot.get_current_fig_manager().canvas
canvas.draw()
pil_image = PIL.Image.fromstring('RGB', canvas.get_width_height(), canvas.tostring_rgb())
pyplot.close()

# The resulting PIL.Image object can be used for lots of useful things in Python.
# For example, you can save it as a PNG file, or get a string of the PNG's contents.
buffer = BytesIO()
pil_image.save(buffer, 'PNG')
buffer.seek(0)
png_bytes = buffer.read()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
