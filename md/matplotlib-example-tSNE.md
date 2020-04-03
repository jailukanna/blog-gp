---
title: matplotlib example tSNE (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'tSNE'

Functions in program: 
* `def plot_SNE(X,Y):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import random`

## python tSNE

Python matplotlib example: tSNE

```python
import random
import matplotlib.pyplot as plt
from matplotlib.lines import Line2D
from sklearn.manifold import TSNE

def plot_SNE(X,Y):
	X_embedded = TSNE(n_components=2).fit_transform(X)
	
	number_of_colors = len(np.unique(Y))
	color_palette = np.array(["#"+''.join([random.choice('0123456789ABCDEF') for j in range(6)]) for i in range(number_of_colors)])
	colors = list(color_palette[Y])
	# Also try this -- https://sashat.me/2017/01/11/list-of-20-simple-distinct-colors/

	f, axarr = plt.subplots(1, figsize=(15,15))
	axarr.scatter(X_embedded[:,0], X_embedded[:,1], color=colors) 
	
	axarr.set_title('t-SNE Embedding', fontsize=20)
	legend_elements = []
	for i in range(len(color_palette)):
		legend_elements.append(
			Line2D([0], [0], marker='o', color=colors[-1], label=str(i+1), 
				   markerfacecolor=color_palette[i], markersize=20)
		)
	plt.legend(handles=legend_elements, loc='upper left')
	plt.rc('legend', fontsize=30)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
