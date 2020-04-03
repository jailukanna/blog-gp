---
title: matplotlib example emotions from openface (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'emotions from openface'

Functions in program: 
* `def get_ausc(ints):`
* `def get_ausr(ints):`

Modules used in program: 
* `import seaborn as sns`
* `import matplotlib.animation as animation`
* `import matplotlib.pylab as plt`
* `import matplotlib`
* `import numpy as np`
* `import pandas as pd`

## python emotions from openface

Python matplotlib example: emotions from openface

```python
#!/usr/bin/env python
# coding: utf-8

# In[1]:


import pandas as pd
import numpy as np
import matplotlib
import matplotlib.pylab as plt
import matplotlib.animation as animation
get_ipython().run_line_magic('matplotlib', 'inline')
get_ipython().run_line_magic('matplotlib', 'notebook')
import seaborn as sns
sns.set(context="talk")


# In[2]:


sti = "./processed/3zmhiYso.csv"
df = pd.read_csv(sti) #
df = df.set_index(" timestamp")


# In[3]:


def get_ausr(ints):
    return [" AU%02d_r"% i for i in ints] 
def get_ausc(ints):
    return [" AU%02d_c"% i for i in ints]


# In[4]:


# .values tager værdierne ud til de specifikke AUs for hver følelse, som dereter bliver summereret rækkevis.
df["happiness"] = df[get_ausr([6,12])].values.mean(1)
df["sadnes"] = df[get_ausr([1,4,15])].values.mean(1)
df["surprise"] = df[get_ausr([1,2,5,26])].values.mean(1)
df["fear"] = df[get_ausr([1,2,4,5,7,20,26])].values.mean(1)
df["anger"] = df[get_ausr([1,2])].values.mean(1)
df["disgust"] = df[get_ausr([1,15])].values.mean(1)
df["contempt"] = df[get_ausr([12,14])].values.mean(1)


# In[5]:


df[
    ["happiness","sadnes","surprise","fear","anger","disgust","contempt"]
].rolling(100).mean().plot(figsize=(20,10))


# In[6]:


df[get_ausr([1,2,4,5,7,9,12,14,15,20,23,26])[:]].rolling(100).mean().plot(figsize=(20,10))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
