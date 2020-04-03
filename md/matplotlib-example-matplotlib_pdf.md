---
title: matplotlib example matplotlib pdf (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'matplotlib pdf'


Modules used in program: 
* `import matplotlib as mpl`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import pandas as pd`
* `import sys, re, glob, os`

## python matplotlib pdf

Python matplotlib example: matplotlib pdf

```python
#if used in jupyter notebook
%matplotlib inline

import sys, re, glob, os
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import matplotlib as mpl
from matplotlib.backends.backend_pdf import PdfPages

#so the font can be handle by illustrator
mpl.rcParams['pdf.fonttype'] = 42
plt.style.use('ggplot')

with PdfPages('1.pdf') as pdf:
    #do some plot
    
    plt.plot([0,1], [0,1])
    pdf.savefig()
    plt.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
