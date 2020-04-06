---
title: mysql example generateHeatmap (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'generateHeatmap'


Modules used in program: 
* `import sys`
* `import matplotlib.pyplot as plt`
* `import seaborn as sns`
* `import pandas as pd`

## python generateHeatmap

Python mysql example: generateHeatmap

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import sys

if len(sys.argv)<2:
    print("No tolerance specified")
    exit()

tolerance = sys.argv[1]

df = pd.read_csv('csv/heatmapData_{}.csv'.format(tolerance))
# Convert values into float
df = df[df.columns].astype(float)
# Dataframe must be pivoted before passing it to sns.heatmap()
result = df.pivot(index='Chars', columns='NumberOfSnippets', values='DuplicateFraction')

cmap = plt.get_cmap("Blues")
cmap.set_bad(color='white', alpha=0.5)
# Mask all NaN results, i.e. missing data. These will show as white in the heatmap.
mask = pd.isnull(result)
ax = sns.heatmap(result, vmin=0.0, vmax=1, mask=mask, annot=False, fmt="g",
                 cmap=cmap, linewidths=0.7, linecolor="white", square=True,
                 cbar_kws={'label': 'Duplicate frequency'})

# Colorbar customization
cbar = ax.collections[0].colorbar
cbar.set_ticks([0, .25, .5, .75, 1])
cbar.set_ticklabels(['0%', '25%', '50%', '75%', '100%'])

# Smaller font sizes for axis labels
for tick in ax.xaxis.get_major_ticks():
    tick.label.set_fontsize(9)

for tick in ax.yaxis.get_major_ticks():
    tick.label.set_fontsize(9)

file=""
if tolerance=="1":
    plt.title("Excluding complete clones")
    file="excluding_clones.pdf"
elif tolerance=="2":
    plt.title("Including complete clones")
    file="including.pdf"

plt.gcf().subplots_adjust(bottom=0.23)
ax.set_ylabel("Number of characters", fontsize=16)
ax.set_xlabel("Number of snippets", fontsize=16)

# Invert y axis so that higher values appear at the top
ax.invert_yaxis()

plt.savefig("plots/{}".format(file))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
