---
title: matplotlib example init packages (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'init packages'


Modules used in program: 
* `import tensorflow as tf`
* `import sklearn.metrics`
* `import seaborn as sns`
* `import pandas as pd`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import h5py`
* `import sys`
* `import shutil`
* `import random`
* `import platform`
* `import os`

## python init packages

Python matplotlib example: init packages

```python
# Code Block 1
!pip install --upgrade pip==19.0.3
print('--> Done with pip upgrade.')
!pip uninstall -y tensorflow numpy pandas matplotlib h5py sklearn tqdm bleach
print('--> Done with uninstalls.')
!pip install tensorflow==1.12 numpy pandas seaborn h5py sklearn tqdm bleach==2.1.2 matplotlib==2.0 seaborn
!pip install matplotlib==2.2.4
print('--> Done with installs.')

# Code Block 2
import os
import platform
import random
import shutil
import sys

import h5py
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
import sklearn.metrics
import tensorflow as tf
from tensorflow.python.saved_model import tag_constants
from tqdm import tqdm_notebook as tqdm


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
