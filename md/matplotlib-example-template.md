---
title: matplotlib example template (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'template'

Functions in program: 
* `def main():`

Modules used in program: 
* `import argparse`
* `import math`
* `import pandas as pd`
* `import numpy as np`
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`
* `import matplotlib`

## python template

Python matplotlib example: template

```python
%matplotlib inline
import matplotlib
matplotlib.use('agg')
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import pandas as pd
from scipy import io
import math


sns.set_style("ticks")
sns.set_context("paper", font_scale=2.0)


import argparse

def main():
    parser = argparse.ArgumentParser(description='test program')

    parser.add_argument('--version', action='version', version='%(prog)s 20XX-XX-XX')
    parser.add_argument('-o', metavar = 'o', default = None,
                        help = 'output file')
    parser.add_argument('-p', metavar = 'p', type=float,default = 0.001,
                        help = 'param (default: 0.001)')
    args = parser.parse_args()

    print(args.O)

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
