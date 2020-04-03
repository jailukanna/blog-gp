---
title: matplotlib example keras remote training 0 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'keras remote training 0'


Modules used in program: 
* `import talos as ta`
* `import keras`
* `import tensorflow as tf`
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import pandas as pd`
* `import numpy as np`
* `import botocore`
* `import boto3`
* `import pwd`
* `import os`

## python keras remote training 0

Python matplotlib example: keras remote training 0

```python
# Setup control options
control = {
    'pip': False,                         # install packages in this notebook
    'experiment_name': 'wine_quality_1',  # for talos scan
    'run_scan': True,                     # whether to execute scan
    'upload_results': True,               # upload results to S3 or use local file
    'download_results': True,             # download results from S3 or use local file
    's3bucket': 'keras-remote-training'   # can be None if using only local files
}

results_filename = control['experiment_name'] + '.csv'
deploy_filename = control['experiment_name'] + '.zip'

# Optional install of packages for standalone notebooks
if control['pip'] == False:
    print('Skipping install of python packages')
else:
    !pip install -r requirements.txt # IPython only
    

# Start imports
import os
import pwd
from collections import OrderedDict

import boto3
import botocore

import numpy as np
import pandas as pd
from sklearn.utils.class_weight import compute_class_weight
from sklearn.metrics import classification_report
from sklearn.metrics import roc_auc_score

import matplotlib
if pwd.getpwuid(os.getuid())[0] == 'kerasdeploy':
    print('Using agg backend for matplotlib')
    matplotlib.use("agg")
import matplotlib.pyplot as plt

import seaborn as sns

import tensorflow as tf
import keras

import talos as ta
from talos import model as ta_model
from talos.metrics.keras_metrics import precision, recall, f1score, matthews

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
