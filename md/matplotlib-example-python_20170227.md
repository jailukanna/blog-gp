---
title: matplotlib example python 20170227 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'python 20170227'


Modules used in program: 
* `import matplotlib`
* `import bisect`
* `import enum`
* `import json`
* `import collections`
* `import re`
* `import sys`
* `import colorlog`
* `import subprocess as sp`
* `import itertools as it`
* `import pandas as pd`
* `import scipy as sc`
* `import numpy as np`

## python python 20170227

Python matplotlib example: python 20170227

```python
%matplotlib inline

import numpy as np
import scipy as sc
import pandas as pd
import itertools as it
import subprocess as sp
import colorlog
import sys
import re
import collections
import json
import enum
import bisect
import matplotlib
matplotlib.use('agg')
from matplotlib import pyplot as plt


logger = colorlog.getLogger()
logger.setLevel(colorlog.colorlog.logging.DEBUG)
handler = colorlog.StreamHandler()
handler.setFormatter(colorlog.ColoredFormatter())
logger.addHandler(handler)

logger.debug("Debug message")
logger.info("Information message")
logger.warning("Warning message")
logger.error("Error message")
logger.critical("Critical message")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
