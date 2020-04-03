---
title: matplotlib example annualvscumulativeco2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'annualvscumulativeco2'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib.font_manager as fm`
* `import matplotlib as  mpl`

## python annualvscumulativeco2

Python matplotlib example: annualvscumulativeco2

```python
python
import matplotlib as  mpl
mpl.rcParams['font.family']='Avenir-Light'
mpl.rcParams['font.size']=16

import matplotlib.font_manager as fm
import matplotlib.pyplot as plt
import numpy as np

prop=fm.FontProperties(fname='/System/Library/Fonts/Avenir.ttc')

fig=plt.figure()
ax1 = fig.add_axes((.1,.25,.6,.6))
txt='''Source: EIA International Energy Outlook 2013, \nMauna Loa Atmospheric CO2 measurements, EON projections'''

ax = plt.gca()
ax.yaxis.grid(True)

tenthAnnEmm=[0.865412, 1.305452, 1.958178, 3.0032729999999996, 3.4176439999999997, 3.8613509999999995, 4.763433, 5.97721, 9.420523, 14.862350999999999, 19.493771999999996, 22.555717, 23.000717, 22.0, 20.0, 18.0, 13.0, 10.0, 5.97721, 3.4176439999999997, 2.0, 0.865412, 0.0]
yearraw=range(1880,2101)
years=yearraw[0::10]
ax1.plot(years, tenthAnnEmm, 'cornflowerblue',linewidth=1.5)
ax1.set_xlabel('Year',fontproperties=prop,fontsize=18)

ax1.set_ylim([0,200])
ax1.set_ylabel('Annual Emissions (gigatonnes of CO2)', color='cornflowerblue',fontproperties=prop,fontsize=18)
for tl in ax1.get_yticklabels():
    tl.set_color('cornflowerblue')

yticks = ax1.yaxis.get_major_ticks()
yticks[0].label1.set_visible(False)

plt.xticks(rotation=40,fontproperties=prop)

ax2=ax1.twinx()
tenthcuml=[619.1909999999999, 626.646, 630.054, 638.361, 645.39, 654.3359999999999, 661.1519999999999, 661.7909999999999, 673.7189999999999, 691.824, 720.7068, 751.9113, 802.158, 877.56, 937.1999999999999, 1003.2299999999999, 1067.1299999999999, 1124.6399999999999, 1171.5, 1207.71, 1229.01, 1239.6599999999999, 1256.7]
ax2.plot(years,tenthcuml,'firebrick',linewidth=1.5)
ax2.set_ylabel('CO2 concentration (gigatonnes of CO2)', color='firebrick',fontproperties=prop,fontsize=18)
for tl in ax2.get_yticklabels():
    tl.set_color('firebrick')

y2ticks = ax2.yaxis.get_major_ticks()
y2ticks[0].label2.set_visible(False)

plt.yticks(fontproperties=prop)
plt.title('Cumulative and Annual Emissions\n',fontproperties=prop,fontsize=22,fontweight='bold')
plt.tight_layout()

fig.text(.1,.01,txt,fontproperties=prop)

plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
