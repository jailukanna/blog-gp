---
title: matplotlib example plot log cases (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot log cases'

Functions in program: 
* `def get_sorted_aggregate(data, threshold):`

Modules used in program: 
* `import matplotlib.font_manager as font_manager`
* `import datetime`
* `import matplotlib.style as style`
* `import seaborn as sns`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`
* `import pandas as pd`

## python plot log cases

Python matplotlib example: plot log cases

```python
import pandas as pd
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib.style as style
import datetime
d = datetime.datetime.today()
print(('Current date and time:', d))
print()
style.use('ggplot')
from cycler import cycler
colors = ['#3498db', '#6ab188', '#e74c3c', '#209cb8', '#95a5a6', '#f541f3', '#abc125', '#ff9d46']
default_cycler = (cycler(color=colors))
plt.rc('axes', prop_cycle=default_cycler)
matplotlib.rcParams.update({'font.size': 14})

# Set the font properties (can use more variables for more fonts)
import matplotlib.font_manager as font_manager
font_path = '/Library/Fonts/UbuntuMono-Regular.ttf'
prop = font_manager.FontProperties(fname=font_path)
matplotlib.rcParams['font.family'] = prop.get_name()

def get_sorted_aggregate(data, threshold):
    aggregated = data.groupby('Country/Region').sum()
    current_cases_sorted = aggregated.loc[:, data.columns[-1]].sort_values(ascending=False)
    sorted_over_hundred = list(current_cases_sorted.loc[current_cases_sorted>threshold].index)
    return aggregated.loc[sorted_over_hundred]

orig_cases = pd.read_csv('../csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Confirmed.csv').drop(columns=['Lat', 'Long'])
orig_deaths = pd.read_csv('../csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Deaths.csv').drop(columns=['Lat', 'Long'])
agg_cases = get_sorted_aggregate(orig_cases, 100)
agg_deaths = get_sorted_aggregate(orig_deaths, 0)

labels = {'Italy': [25,55000], 'Iran': [24,20000], 'Spain': [17, 24000], 'Korea, South': [26, 12000], 'Germany': [18,3300], 'France': [20,5000], 'US': [15.5,2000], 'Portugal': [11,800]}
f,ax = plt.subplots(figsize=(6,5))
for color, country in zip(colors, ['Italy', 'Iran', 'Spain', 'Korea, South', 'Germany', 'France', 'US', 'Portugal']):
    this_country = agg_cases.loc[country]
    cases = this_country.values
    cases = cases[(cases>100)]
    ax.plot(cases, 'o-', label=country)
    ax.text(labels[country][0], labels[country][1], '{}'.format(country), color=color, ha='center', va='center')
ax.axhline(y = 100, color = 'black', linewidth = 1.3)
ax.text(0.01,.97,'Cumulative number of cases, by number of days since 100th case', fontweight="bold", fontsize=13, ha='left', va='top', transform=f.transFigure)
ax.set_xlabel('days since >100 cases', fontsize=14)
ax.set_ylabel('confirmed cases (log-scale)', fontsize=14)
ax.text(0.01,0.001,'Source: CSSE Johns Hopkins University (https://github.com/CSSEGISandData).\nData updated March {}, 08:00 GMT (commit: a9f182afe873ce7e65d2307fcf91013c23a4556c)'.format(d.day), fontsize=7, ha='left', va='bottom', transform=f.transFigure)
ax.set_yscale('log')
#ax.set_ylim([10,100000])
ax.set_yticks([100,1000,10000,100000])
ax.set_yticklabels([100,1000,10000,100000])
sns.despine(trim=True, ax=ax)
f.tight_layout()
f.savefig('cases_log_{}.png'.format(d.strftime('%d-%m-%Y')), dpi=300)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
