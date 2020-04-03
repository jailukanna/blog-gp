---
title: matplotlib example energyintensity (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'energyintensity'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib.font_manager as fm`
* `import matplotlib as  mpl`

## python energyintensity

Python matplotlib example: energyintensity

```python
python

#This just lets the interactive graph you see have the correct font
import matplotlib as  mpl
mpl.rcParams['font.family']='Avenir-Light'
mpl.rcParams['font.size']=16

import matplotlib.font_manager as fm
import matplotlib.pyplot as plt

fig=plt.figure()
ax = fig.add_axes((.15,.25,.6,.55))
txt='''Source: EIA Annual Energy Outlook 2013 & EON projections'''
ax = plt.gca()
ax.yaxis.grid(True)

yearsraw=range(2005,2051)
years=yearsraw[0::5]
zero=[0.008288123,0.00784054,0.00784054,0.00784054,0.00784054,0.00784054,0.00784054,0.00784054,0.00784054,0.00784054]
plusone=[0.008288123,0.00784054, 0.0079189454, 0.007998134854, 0.00807811620254, 0.0081588973645654, 0.008240486338211055, 0.008322891201593165, 0.008406120113609096, 0.008490181314745186]
minusone=[0.008288123,0.00784054,0.0077621346, 0.0076845132539999994, 0.00760766812146, 0.0075315914402454, 0.007456275525842946, 0.007381712770584516, 0.0073078956428786705, 0.007234816686449884]
plustwo=[0.008288123,0.00784054, 0.0079973508, 0.008157297816000001, 0.008320443772320002, 0.008486852647766401, 0.008656589700721728, 0.008829721494736162, 0.009006315924630885, 0.009186442243123502]
minustwo=[0.008288123,0.00784054,0.0076837292, 0.007530054616000001, 0.007379453523680001, 0.0072318644532064005, 0.0070872271641422725, 0.006945482620859427, 0.006806572968442238, 0.006670441509073394]
#Maybe convert to smaller sigfigs on y values for formatting purposes?


ax.plot(years,zero,'firebrick',linewidth=1.5,label='0%')
ax.plot(years,plusone,'goldenrod',linewidth=1.5,label='+1%')
ax.plot(years,minusone,'cornflowerblue',linewidth=1.5,label='-1%')
ax.plot(years,plustwo,'yellowgreen',linewidth=1.5,label='+2%')
ax.plot(years,minustwo,'darkolivegreen',linewidth=1.5,label='-2%')

#This, however, will dictate the font of the graph that you save
prop=fm.FontProperties(fname='/System/Library/Fonts/Avenir.ttc')

plt.legend(fontsize='smaller',loc='upper left',ncol=3)
yticks = ax.yaxis.get_major_ticks()
yticks[0].label1.set_visible(False)
plt.xlabel('Year',fontproperties=prop,fontsize=18)
plt.ylabel('Energy Intensity (Joules/billions of 2005 USD)',fontproperties=prop,fontsize=18)
plt.title('Energy Intensity Projections\n',fontproperties=prop,fontsize=22,fontweight='bold')
plt.xticks(fontproperties=prop,rotation=40)
plt.yticks(fontproperties=prop)
plt.gca().get_xaxis().get_major_formatter().set_useOffset(False)
plt.tight_layout()
fig.text(.1,.07,txt,fontproperties=prop)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
