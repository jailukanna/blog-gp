---
title: matplotlib example sparkify 1 import libs (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'sparkify 1 import libs'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import seaborn as sns`
* `import numpy as np`
* `import pyspark.sql.functions as F`

## python sparkify 1 import libs

Python matplotlib example: sparkify 1 import libs

```python
# import libraries
from pyspark import SparkConf, SparkContext
from pyspark.sql import SparkSession, Window
from pyspark.sql.functions import count, when, isnan, isnull, desc_nulls_first, desc, \
    from_unixtime, col, dayofweek, dayofyear, hour, to_date, month
import pyspark.sql.functions as F
from pyspark.ml.feature import OneHotEncoderEstimator, StringIndexer, VectorAssembler, StandardScaler, MinMaxScaler
from pyspark.ml.classification import DecisionTreeClassifier, RandomForestClassifier

# sc = SparkContext(appName="Project_workspace")

# This is another way of doing (If you are running local cluster setMaster="local")
# configure = SparkConf().setAppName("name").setMaster("IP Address")
# sc = SparkContext(conf=configure)
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
