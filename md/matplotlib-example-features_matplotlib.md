---
title: matplotlib example features matplotlib (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'features matplotlib'


Modules used in program: 
* `import matplotlib.dates as mdates`
* `import matplotlib.pyplot as plt`

## python features matplotlib

Python matplotlib example: features matplotlib

```python
# import modules and tools
import matplotlib.pyplot as plt
import matplotlib.dates as mdates

# SOURCE OF COLOURS: http://matplotlib.org/examples/color/named_colors.html
from matplotlib import colors as mcolors
lname_colors = mcolors.cnames.keys()

# create objects
fig, ax = plt.subplots()
#
fig = plt.figure(figsize=(10,5))
ax = fig.gca()

# create axes
ax = fig.add_axes([0.1,0.1,0.8,0.8])

# plot and set legend
ax.scatter(x1,y1,label = "label 1 of legend")
ax.scatter(x2,y2,label = "label 2 of legend")
ax.legend(loc='best', # 'best', 'upper right','upper left', etc
          fontsize=9,
          shadow=True)

# SET TEXT LABELS: option 1
for i, txt in enumerate(lname):
    ax.annotate(txt, (x[i]+0.002,y[i]))
# SET TEXT LABELS: option 2
from matplotlib.font_manager import FontProperties
font = FontProperties()
font.set_family('sans-serif') # ['serif', 'sans-serif', 'cursive', 'fantasy', 'monospace']
font.set_style('italic')
font.set_weight('semibold') #['light', 'normal', 'medium', 'semibold', 'bold', 'heavy', 'black']
font.set_size(28)
plt.text(0., 0.90, 'LABEL TO BE WRITTEN', fontproperties=font,color = 'red')




# Save the default tick positions, so we can reset them...
locs, labels = plt.xticks()
# resolution of axis ticks
plt.xticks(range(0,22,1),fontsize='small',rotation='vertical' #'horizontal')
plt.yticks(range(-20000,50000,2500),fontsize='small',rotation='vertical' #'horizontal')

# limits of axes
ax.set_xlim([0,22])
ax.set_ylim([-20000,50000])

# format dates in x axis
myFmt = mdates.DateFormatter('%a %d,%H:%M' )
ax.xaxis.set_major_formatter(myFmt)
plt.xticks(rotation='vertical', fontsize='small')

# format general labels in x axis
plt.xticks(list_position,fontsize='small',rotation='vertical' #'horizontal')
ax.set_xticklabels(list_labels)
# format general labels in x axis (without plt object)
ax.set_xticks(list_position)
ax.set_xticklabels(list_labels, rotation='rotation',fontsize='small')

# customize x/y tick labels (without change values)
for tick in ax.xaxis.get_major_ticks():
                tick.label.set_fontsize(14) 
                tick.label.set_rotation('vertical')

# hide x ticks labels
plt.tick_params(axis='x',which='both',bottom='off',top='off',labelbottom='off')


# set title    
plt.title("title")
ax.set_title("title")

# titles in japanese (for Ubuntu)
from matplotlib.font_manager import FontProperties
from matplotlib import rcParams
font_path = '/usr/share/fonts/truetype/takao-gothic/TakaoPGothic.ttf'
font_prop = FontProperties(fname=font_path)
rcParams['font.family'] = font_prop.get_name()
ax.set_title(u'秋田　天秤野発電所',fontsize=20)


# set subtitle
fig.suptitle("subtitle")

# axes labels
plt.xlabel("lable x")
plt.ylabel("lable y")
ax.set_xlabel("lable x")
ax.set_ylabel("lable y")
    
# plot grid axis
ax.xaxis.grid(True)
ax.yaxis.grid(True)

# adjust space of chart
plt.subplots_adjust(bottom=0.2) 

# display chart by screen
plt.show()

# save chart into output file
fig.savefig(path_output,bbox_inches='tight',transparent=False)
plt.savefig(path_output,bbox_inches='tight',transparent=False)

# close plot
plt.cla()   # Clear axis
plt.clf()   # Clear figure
plt.close() # Close a figure window




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
