---
title: matplotlib example plot histogram (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot histogram'

Functions in program: 
* `def fmt(data):`
* `def get_module_entropies(path):`
* `def load_alpha_data(log_file, prefix='data'):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy`
* `import collections`
* `import seaborn`
* `import pandas`
* `import matplotlib`
* `import glob`
* `import json`
* `import os`
* `import sys`
* `import glob`

## python plot histogram

Python matplotlib example: plot histogram

```python
import glob
import sys
import os
import json
import glob
import matplotlib
import pandas
import seaborn
import collections
# matplotlib.use('agg')
from matplotlib import pyplot
%matplotlib inline
matplotlib.style.use('ggplot')
import numpy

colors = ['salmon', 'steelblue']


def load_alpha_data(log_file, prefix='data'):
  data = [[], [], []]
  with open(log_file) as log:
    for line in log:
      if line.startswith(prefix):
        num = int(line[5])
        row = [float(x) for x in line[7:].split()]
        data[num].append(row)
  return data

def get_module_entropies(path):
  entropies = []
  for f in glob.glob(path):
    probs = load_alpha_data(f, 'data')
    for i in [0, 1]:
      p_x = probs[i][-1][0]
      p_y = probs[i][-1][2]
      p_x, p_y = numpy.exp([p_x, p_y]) / numpy.exp([p_x, p_y]).sum()
      entropy = max(p_x, p_y) / min(p_x, p_y) 
      #-p_x * numpy.log(p_x) -  p_y * numpy.log(p_y)
      entropies.append(entropy)
  return entropies

entropies_x = get_module_entropies('induction/shnmn_fixed_tree_find64_learnt_param_variety_18_sharper_softmax_100k/slurm-*.out')
entropies_y = get_module_entropies('induction/shnmn_fixed_tree_find64_learnt_param_variety_1_sharper_softmax_100k/slurm-*.out')
def fmt(data):
  return " ".join(["{:.3f}".format(d) for d in data])
print(fmt(sorted(entropies_x)))
print(fmt(sorted(entropies_y)))
import matplotlib.pyplot as plt

#df1 = pandas.DataFrame({'Easy' : entropies_x,'Hard' : entropies_y})

#df1.hist(alpha=0.4) #plas.hist(alpha=0.4)
#import seaborn as sns

bins = [0,2,4,6,8,10,12,14,16]
pyplot.hist([entropies_y, entropies_x], ls='dotted', color=colors, label = ['1 rhs/lhs', '18 rhs/lhs'])

fs = 18
ncol=2

pyplot.legend(
              ncol=ncol,
              prop={'size': 12}, fancybox=True, framealpha=0.5)


pyplot.ylabel('#Runs', fontsize=fs)
pyplot.xlabel('Sharpness', fontsize=fs)



ax = pyplot.gca()
ax.xaxis.label.set_color('black')
ax.yaxis.label.set_color('black')

ax.xaxis.get_offset_text().set_size(fs)
for item in (ax.get_xticklabels() + ax.get_yticklabels()):
    item.set_fontsize(fs)
    item.set_color('black')


pyplot.savefig('sharpness_hardvseasy.png')
pyplot.show() 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
