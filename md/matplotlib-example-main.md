---
title: matplotlib example main (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'main'

Functions in program: 
* `def main():`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python main

Python matplotlib example: main

```python
from __future__ import unicode_literals
from __future__ import division

import numpy as np
from matplotlib import scale
from matplotlib import pylab
from matplotlib import transforms as mtransforms
from matplotlib.ticker import MultipleLocator
import matplotlib.pyplot as plt


class HydraulicN185Scale(scale.ScaleBase):
    name = 'hydraulic-n-1.85'

    def __init__(self, axis, **kwargs):
        scale.ScaleBase.__init__(self)

    def get_transform(self):
        return HydraulicN185Transform()

    def set_default_locators_and_formatters(self, axis):
        axis.set_major_locator(MultipleLocator(100))
        axis.set_minor_locator(MultipleLocator(10))


class HydraulicN185Transform(mtransforms.Transform):
    input_dims = 1
    output_dims = 1
    is_separable = True

    def transform_non_affine(self, a):
        array = np.array(map(lambda x: max((x / 100) * 1.85, 1) * x, a))
        return array

    def inverted(self):
        return self


scale.register_scale(HydraulicN185Scale)


def main():
    plt.rc('lines', linewidth=2, color='r')
    plt.rc('grid', linestyle="-", color='black')

    first_point = (0, 30)
    second_point = (600, 0)
    third_point = (10, 10)
    forth_point = (300, 10)

    # -------
    # First line
    points = [
        first_point,
        second_point,
    ]

    x = map(lambda x: x[0], points)
    y = map(lambda x: x[1], points)
    plt.plot(x, y, '-', color='blue')

    # --------
    # Second line (dashed)

    points = [
        third_point,
        forth_point,
    ]

    x = map(lambda x: x[0], points)
    y = map(lambda x: x[1], points)
    plt.plot(x, y, '--', color='red')
    plt.scatter(x, y, color='red')

    # --------

    pylab.xlim([0, 1000])
    pylab.ylim([0, 175])
    gca = plt.gca()
    gca.set_xscale('hydraulic-n-1.85')
    gca.yaxis.set_major_locator(MultipleLocator(10))
    gca.yaxis.set_minor_locator(MultipleLocator(1))

    plt.tick_params('both', which='both', left='on', bottom='on', right='off', top='off', color='black')
    plt.tick_params('both', which='minor', width=1, length=3)
    plt.ylabel('PSI')
    plt.xlabel('GPM')
    plt.title('Hydraulic N 1.85')
    plt.grid(True)

    plt.show()


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
