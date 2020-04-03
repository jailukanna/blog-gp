---
title: matplotlib example init (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'init'


Modules used in program: 
* `import matplotlib.font_manager`
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`
* `import collections`
* `import json`
* `import scipy`
* `import numpy as np`
* `import pandas as pd`

## python init

Python matplotlib example: init

```python
# -*- coding: utf_8 -*-  
%matplotlib inline

import pandas as pd
import numpy as np
import scipy
import json
import collections
import matplotlib.pyplot as plt
from tqdm import tqdm_notebook as tqdm
from tqdm import trange
import seaborn as sns
import matplotlib.font_manager
from matplotlib.font_manager import FontProperties
from matplotlib.backends.backend_pdf import PdfPages
font_path = '/Library/Fonts/RictyDiminishedDiscord-Regular.ttf'
font_prop = matplotlib.font_manager.FontProperties(fname=font_path)
matplotlib.rcParams['font.family'] = font_prop.get_name()
from IPython.core.display import display

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
