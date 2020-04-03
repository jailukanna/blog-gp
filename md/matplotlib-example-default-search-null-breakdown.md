---
title: matplotlib example default-search-null-breakdown (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'default-search-null-breakdown'


Modules used in program: 
* `import datetime as dt`
* `import plotly.plotly as py`
* `import numpy as np`
* `import pandas as pd`
* `import matplotlib.pyplot as plt`
* `import ujson as json`

## python default-search-null-breakdown

Python matplotlib example: default-search-null-breakdown

```python

# coding: utf-8

# ### Bug 1249288 - Breakdown of null values for defaultSearch

# In[1]:

import ujson as json
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import plotly.plotly as py
import datetime as dt
from uuid import UUID

from moztelemetry import get_pings, get_pings_properties, get_one_ping_per_client, get_clients_history

get_ipython().magic(u'pylab inline')


# In[2]:

submission_dates = ("20160420", "20160422")
core_pings = get_pings(sc,
                        app="Fennec",
                        channel="beta",
                        doc_type="core",
                        source_version="2",
                        submission_date=submission_dates,
                        fraction=1.0)


# In[3]:

pings_count = core_pings.count()
pings_count


# ### How many different clients are we seeing?

# In[4]:

one_per_client = get_one_ping_per_client(core_pings)
num_clients = one_per_client.count()
num_clients


# ### Find pings which submit the distribution field

# In[6]:

distribution_pings = core_pings.filter(lambda p: p.get("distribution", None) != None)                               .collect()
len(distribution_pings)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
