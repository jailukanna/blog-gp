---
title: matplotlib example Check%2520moztelemetry%2520types (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Check%2520moztelemetry%2520types'

Functions in program: 
* `def extract(pings):`

Modules used in program: 
* `import datetime as dt`
* `import plotly.plotly as py`
* `import numpy as np`
* `import pandas as pd`
* `import matplotlib.pyplot as plt`
* `import ujson as json`

## python Check%2520moztelemetry%2520types

Python matplotlib example: Check%2520moztelemetry%2520types

```python

# coding: utf-8

# ## Check moztelemetry types

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


# We get all the pings on Beta, after bug 1293222 landed.

# In[15]:

def extract(pings):
    subset = get_pings_properties(pings, ["meta/clientId",
                                          "meta/documentId",
                                          "payload/processes/parent",
                                          "payload/processes/parent/events"])
    return subset

pings = get_pings(sc,
                  app="Firefox",
                  channel="nightly",
                  doc_type="main",
                  submission_date="20161216",
                  build_id="20161210030206",
                  fraction=1.0)
extracts = extract(pings)


# In[24]:

ping = extracts.filter(lambda p: p["meta/documentId"] == "0aa23e4c-b779-f544-9c90-5193a4a44c59")               .first()


# On my client i can clearly see that `payload/processes/parent/events` is `[]`.
# 
# Extracting it here, it became a `dict` type instead of the expected `list`:

# In[25]:

ping["payload/processes/parent/events"]


# In[26]:

tmp = ping["payload/processes/parent"]
tmp["gc"] = {} # Readability ...
tmp["scalars"] = {} # Readability ...
tmp


# Skipping `get_pings_properties()`, the result is the same:

# In[27]:

ping = pings.filter(lambda p: p["meta"]["documentId"] == "0aa23e4c-b779-f544-9c90-5193a4a44c59")            .first()


# In[28]:

ping["payload"]["processes"]["parent"]["events"]



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
