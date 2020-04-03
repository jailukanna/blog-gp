---
title: matplotlib example importdata (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'importdata'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import pandas as pd`
* `import numpy as np`

## python importdata

Python matplotlib example: importdata

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from pandas.plotting import register_matplotlib_converters ##needed to properly use datetime in plots
register_matplotlib_converters()  ##needed to properly use datetime in plots
from datetime import datetime,timedelta
from sklearn.metrics import mean_squared_error
from scipy.optimize import curve_fit
from scipy.optimize import fsolve


giturl = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/4b8141783d481d09efe8104cb29f8966df4a6354/csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Confirmed.csv'
giturl_deaths = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/4b8141783d481d09efe8104cb29f8966df4a6354/csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Deaths.csv'

casestr='Cases' #'Cases', 'Deaths'
choosemodel = 'Logistic'  #'Logistc'

if casestr=='Cases':
    curl = giturl
elif casestr=='Deaths':
    curl = giturl_deaths
    
corona_cases_df=pd.read_csv(curl, index_col=[0,1,2,3])  #   use first four columns as multiindex

corona_cases_df.columns.values
>>> 
array(['1/22/20', '1/23/20', '1/24/20', '1/25/20', '1/26/20', '1/27/20',
       '1/28/20', '1/29/20', '1/30/20', '1/31/20', '2/1/20', '2/2/20',
       '2/3/20', '2/4/20', '2/5/20', '2/6/20', '2/7/20', '2/8/20',
       '2/9/20', '2/10/20', '2/11/20', '2/12/20', '2/13/20', '2/14/20',
       '2/15/20', '2/16/20', '2/17/20', '2/18/20', '2/19/20', '2/20/20',
       '2/21/20', '2/22/20', '2/23/20', '2/24/20', '2/25/20', '2/26/20',
       '2/27/20', '2/28/20', '2/29/20', '3/1/20', '3/2/20', '3/3/20',
       '3/4/20', '3/5/20', '3/6/20', '3/7/20', '3/8/20', '3/9/20',
       '3/10/20', '3/11/20', '3/12/20'], dtype=object)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
