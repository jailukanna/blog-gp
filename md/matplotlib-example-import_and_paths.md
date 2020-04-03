---
title: matplotlib example import and paths (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'import and paths'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import pandas`
* `import lxml.html`
* `import itertools`
* `import pathlib`
* `import ast`
* `import csv`
* `import json`
* `import datetime`

## python import and paths

Python matplotlib example: import and paths

```python
# import core libraries 
import datetime
import json
import csv
import ast
import pathlib
import itertools
from collections import Counter
from itertools import islice

# import third-party libraries
import lxml.html
import pandas
from pandas.io.json import json_normalize
from pandas import ExcelWriter

# import visualizations
%matplotlib inline
import matplotlib
import matplotlib.pyplot as plt

# set directory path data
twitter_data_dir = pathlib.Path('/Users/adamstueckrath/Desktop/twitter_data/')

# set tweets_json_path path
tweets_json = twitter_data_dir / 'tweets' / 'tweets.json'


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
