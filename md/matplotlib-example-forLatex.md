---
title: matplotlib example forLatex (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'forLatex'

Functions in program: 
* `def plot(data): # data is a numpy array`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`
* `import os`

## python forLatex

Python matplotlib example: forLatex

```python
"""
To save images to embed in latex
"""

import os
import matplotlib as mpl
import matplotlib.pyplot as plt

mpl.rcParams['figure.dpi']= 100  # keep this low, else it takes times to render on matplotlib
mpl.rc("savefig", dpi=300)       # keep this high.
inline_rc = dict(mpl.rcParams)   # for resetting purposes
%matplotlib inline

def plot(data): # data is a numpy array
	f, axarr = plt.subplots(1, figsize=(10,10))
	plt.plot(data[:,0], data[:,1], color='blue', label='MyData', linestyle=':', alpha=1.0, marker='o') # linestyle = ['-', '--', ':'] 

	# Styling
	# mpl.rcParams.update(mpl.rcParamsDefault) # set this since every run needs this
	mpl.rcParams.update(inline_rc)             # set this since every run needs this
	plt.rc('legend', fontsize=20)
	plt.rc('xtick', labelsize=15)
	plt.rc('ytick', labelsize=15)
	# axarr.xaxis.set_major_locator(plt.MaxNLocator(5)) # to limit the number of ticks
	axarr.xaxis.get_label().set_fontsize(20)
	axarr.yaxis.get_label().set_fontsize(20)
	
	plt.xlabel('X Axis')
	plt.ylabel('Y axis')
	plt.title('My Plot', fontsize=30)
	plt.legend(loc='upper left')

	if not os.path.isdir('./images/'):
		os.mkdir('images')
	filename = 'images/image_' + str(datetime.datetime.now().replace(microsecond=0)) + '.png' 
	f.savefig(filename, bbox_inches = 'tight',pad_inches = 0, format='png')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
