---
title: matplotlib example datascience (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'datascience'

Functions in program: 
* `def pygam_plot_signific(gam): `
* `def flatten_index(df):`
* `def deseasonalise(df):`
* `def kalfil(series):`
* `def markdown(string):`

Modules used in program: 
* `import matplotlib as mpl`
* `import matplotlib.colors as colors`
* `import chartsh`
* `import plotly.express as px`
* `import plotly.io as pio`
* `import statsmodels.formula.api as smf`
* `import statsmodels.api as sm`
* `import pymc3 as pm`
* `import altair as alt`
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import requests`
* `import numpy as np`
* `import pandas as pd`

## python datascience

Python matplotlib example: datascience

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
get_ipython().magic(u"%load_ext lab_black")
# %load_ext lab_black
import pandas as pd
from pandas.plotting import scatter_matrix
pd.options.display.float_format = '{:.2f}'.format

import numpy as np

import requests
from splinter import Browser

import matplotlib
import matplotlib.pyplot as plt
import seaborn as sns

from IPython.display import Markdown

def markdown(string):
    display(Markdown(string))

import altair as alt
# alt.renderers.enable('notebook')
from pykalman import KalmanFilter

from fbprophet import Prophet
from matrixprofile import *
import pymc3 as pm

import statsmodels.api as sm
import statsmodels.formula.api as smf
from statsmodels.tsa.seasonal import seasonal_decompose

# import stackprinter
# stackprinter.set_excepthook(style='color')

# %matplotlib inline
get_ipython().run_line_magic('matplotlib', 'inline')
# plt.style.use("seaborn-whitegrid")

sns.set(rc={'figure.figsize':(11.7,8.27)})

def kalfil(series):
    kf = KalmanFilter(initial_state_mean=series.to_list()[0], n_dim_obs=1)
    return kf.em(series.values).smooth(series.values)[0]


# jtplot.reset()
alt.themes.enable("default")
import plotly.io as pio
# pio.renderers.default = "notebook"
import plotly.express as px
import chartsh

def deseasonalise(df):
    """ takes a df, with datetime index and column same as target. Outputs a deseasonalised series """
    df = df.reset_index()
    df.columns = ["ds", "y"]
#         print(tdf.head())
    m = Prophet(yearly_seasonality = True)
    m.fit(df)
    future = m.make_future_dataframe(0)
    forecast = m.predict(future)
    forecast["y"] = df["y"]
    forecast["resid"] = forecast["y"] - forecast["yhat"]
    forecast["deseasonalised"] = forecast["yearly"].mean() + forecast["weekly"].mean() + forecast["resid"] + forecast["trend"]
#     forecast[["deseasonalised", "y", "yhat"]].plot(subplots = True, figsize = (15, 15))
#         plt.show()
#         print(forecast.head())
#     tsdf[x] = forecast["deseasonalised"]
    return forecast["deseasonalised"]

def flatten_index(df):
    df.columns = [' '.join(col).strip() for col in df.columns.values]
    return df

def pygam_plot_signific(gam): 
    for i, (term, val) in enumerate(
    zip(gam.terms, [x < 0.0011 for x in gam.statistics_["p_values"]])
    ):
        if val == True:
            if term.isintercept:
                continue

            XX = gam.generate_X_grid(term=i)
            pdep, confi = gam.partial_dependence(term=i, X=XX, width=0.95)

            plt.figure()
            plt.plot(XX[:, term.feature], pdep)
            plt.plot(XX[:, term.feature], confi, c="r", ls="--")
            plt.title(repr(term))
            plt.show()

pd.plotting.register_matplotlib_converters()
matplotlib.rcParams['figure.figsize'] = (10.0, 8.0)

import matplotlib.colors as colors
import matplotlib as mpl
from cycler import cycler

mpl.style.use('seaborn-notebook')

brand_colors = ["#f58220", "#916bad", "#9cba40", "#33c9fa", "#f22b1c", "#0abdc9"]
cmap = colors.ListedColormap(brand_colors)

# Set the default color cycle
colors = plt.cm.gray(np.linspace(0, 1, 3))
mpl.rcParams["axes.prop_cycle"] = mpl.cycler(color=brand_colors)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
