---
title: matplotlib example worldelectricityconsumption (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'worldelectricityconsumption'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib.font_manager as fm`
* `import matplotlib.pylab as pylab`
* `import matplotlib as  mpl`

## python worldelectricityconsumption

Python matplotlib example: worldelectricityconsumption

```python
python
#This just lets the interactive graph you see have the correct font
import matplotlib as  mpl
mpl.rcParams['font.family']='Avenir-Light'
mpl.rcParams['font.size']=14
import matplotlib.pylab as pylab
import matplotlib.font_manager as fm
import matplotlib.pyplot as plt
import numpy as np

fig=plt.figure()
ax = fig.add_axes((.15,.25,.65,.55))
txt='''Ref - Reference Case
ME - Medium electrification                  UE - Ultra electrification
MED - Moderate Energy demand        HED - High Energy Demand

Source: EIA Annual Energy Outlook 2013 & EON projections'''

ax = plt.gca()
ax.yaxis.grid(True)

yearsraw=range(2010,2051)
years=yearsraw[0::5]

ref=[245.9688443,268.5438783,295.6900496,319.4145462,342.3352546,364.8452194,380.8029772,396.9308615,403.298151]
ExtEng=[245.9688443,337.9428581,372.104332,401.9598783,430.8039159,459.1310626,480.2464432,496.474792,512.689808]
ModEng=[245.9688443,404.324491,445.1962544,480.916283,515.4261137,549.3175213,575.3662803,595.603769,617.325306]
HiExt=[245.9688443,348.572,413.56,490.364,590.8,768.04,837.4100062,896,952.27216]
HiMod=[245.9688443,417.0415,494.795,586.6855,706.85,918.905,1001.901257,1072,1139.32562]

ax.plot(years,ref,'firebrick',linewidth=1.5,label='EIA Ref')
ax.plot(years,ExtEng,'goldenrod',linewidth=1.5,label='ME, MED')
ax.plot(years,ModEng,'cornflowerblue',linewidth=1.5,label='UE, MED')
ax.plot(years,HiExt,'darkolivegreen',linewidth=1.5,label='ME, HED')
ax.plot(years,HiMod,'lightslategray',linewidth=1.5,label='UE,  HED')

#This, however, will dictate the font of the graph that you save
prop=fm.FontProperties(fname='/Users/admin/Documents/Alla/Avenir.ttc')

plt.xlabel('Year',fontproperties=prop,fontsize=18)
plt.ylabel("World electricity consumption (Exajoules)",fontproperties=prop,fontsize=18)
plt.title("World Electricity Consumption Projections\n",fontproperties=prop,fontsize=22,fontweight='bold')
plt.xticks(fontproperties=prop,rotation=40)
plt.yticks(fontproperties=prop)
yticks = ax.yaxis.get_major_ticks()
yticks[0].label1.set_visible(False)
plt.legend(fontsize='smaller',loc='upper left',ncol=2)

plt.subplots_adjust(bottom=0.13,right=.7)

fig.text(.1,.03,txt,fontproperties=prop)

plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
