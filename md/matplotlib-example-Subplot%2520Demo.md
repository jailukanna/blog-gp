---
title: matplotlib example Subplot%2520Demo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Subplot%2520Demo'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python Subplot%2520Demo

Python matplotlib example: Subplot%2520Demo

```python
# coding: utf-8

# Simple demo with multiple subplots.

import numpy as np
import matplotlib.pyplot as plt

x1 = np.linspace(0.0, 5.0)
x2 = np.linspace(0.0, 2.0)

y1 = np.cos(2 * np.pi * x1) * np.exp(-x1)
y2 = np.cos(2 * np.pi * x2)

plt.subplot(2, 1, 1)
plt.plot(x1, y1, 'yo-')
plt.title('A tale of 2 subplots')
plt.ylabel('Damped oscillation')

plt.subplot(2, 1, 2)
plt.plot(x2, y2, 'r.-')
plt.xlabel('time (s)')
plt.ylabel('Undamped')

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
