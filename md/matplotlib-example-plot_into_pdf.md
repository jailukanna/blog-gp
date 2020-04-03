---
title: matplotlib example plot into pdf (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot into pdf'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import datetime`
* `import matplotlib`

## python plot into pdf

Python matplotlib example: plot into pdf

```python
#!/usr/bin/env python

# Reference:
# [1] http://matplotlib.org/examples/pylab_examples/multipage_pdf.html

# This should be executed before other matplotlib imports
import matplotlib
matplotlib.use('Agg')

import datetime
import numpy as np
from matplotlib.backends.backend_pdf import PdfPages
import matplotlib.pyplot as plt

# Create the PdfPages object to which we will save the pages:
# The with statement makes sure that the PdfPages object is closed properly at
# the end of the block, even if an Exception occurs.
with PdfPages('multipage_pdf.pdf') as pdf:
    plt.figure(figsize=(3, 3))
    plt.plot(range(7), [3, 1, 4, 1, 5, 9, 2], 'r-o')
    plt.title('Page One')
    pdf.savefig()  # saves the current figure into a pdf page
    plt.close()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
