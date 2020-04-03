---
title: matplotlib example eurovision (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'eurovision'


Modules used in program: 
* `import numpy as np`
* `import matplotlib`
* `import matplotlib.colors as colours`
* `import matplotlib.cm as mplcm`
* `import matplotlib.gridspec as gridspec`
* `import matplotlib.pyplot as plt`

## python eurovision

Python matplotlib example: eurovision

```python
#https://imgur.com/gallery/0NoNOHz

import matplotlib.pyplot as plt
import matplotlib.gridspec as gridspec
import matplotlib.cm as mplcm
import matplotlib.colors as colours
import matplotlib
import numpy as np

countries = ["Denmark", "Germany", "UK", "Ukraine", "Australia", "Sweden", "The Netherlands", "Israel", "Austria", "Belgium", "Italy", "Norway", "Ireland", "Greece", "Portugal", "Slovenia", "France", "Poland", "Estonia", "Croatia", "Iceland", "Romania", "Spain"]
costs = [0.13, 0.17, 0.17, 0.30, 0.35, 0.35, 0.45, 0.47, 0.50, 0.50, 0.50, 0.52, 0.60, 0.62, 0.74, 0.84, 0.87, 0.88, 0.96, 1.00, 1.06, 1.25, 1.51]
index = [82.65, 67.89, 67.18, 27.08, 73.87, 70.39, 75.93, 74.86, 72.31, 74.39, 69.68, 104.09, 77.08, 57.64, 50.64, 52.93, 74.83, 38.69, 51.61, 50.05, 112.64, 36.7, 55.43]
score = [(120 - index[i]) / costs[i] for i in range(len(costs))]

gs = gridspec.GridSpec(2, 2)
ax0 = plt.subplot(gs[0, :])
ax1 = plt.subplot(gs[1, 0])
ax2 = plt.subplot(gs[1, 1])

box = ax0.get_position()
ax0.set_title("Cost to Vote Vs. Cost of living index score")
ax0.set_position([box.x0, box.y0, box.width * 0.765, box.height])

ax0.scatter(index, costs)
ax0.set_ylabel("Price (€)")
ax0.set_xlabel("Cost of living index score\nHigher is more expensive")
ax0.yaxis.set_major_formatter(matplotlib.ticker.FormatStrFormatter("%.2f"))
ax0.set_facecolor("#E9E9E9")
cm = mplcm.get_cmap("gist_rainbow")
NUM_COLOURS = len(countries)
ax0.set_color_cycle([cm(1.*i/NUM_COLOURS) for i in range(NUM_COLOURS)])
for i, txt in enumerate(countries):
    ax0.scatter(index[i], costs[i], label = txt)

fit = np.polyfit(index, costs, 1)
fit_fn = np.poly1d(fit)
pmcc = np.corrcoef(costs, index)[0, 1]
ax0.plot(index, fit_fn(index), color = "black", linewidth = 0.5, label = "ρ=%.4f" % pmcc)

ax0.legend(bbox_to_anchor=(1.05, 1), borderaxespad = 0, ncol = 2)

ax1.set_title("Cost to vote by country")
ax1.set_ylabel("Price / €")
ax1.set_facecolor("#E9E9E9")
ax1.yaxis.set_major_formatter(matplotlib.ticker.FormatStrFormatter("%.2f"))
ax1.set_xticks(np.arange(len(costs)))
ax1.set_xticklabels(countries)
ax1.tick_params(axis = "x", rotation = 90)
rects1 = ax1.bar(np.arange(len(costs)), costs, 0.5)

ax2.set_title("Voting value by country")
ax2.set_facecolor("#E9E9E9")
ax2.set_ylabel("Value\n(120 - cost of living index) ÷ Cost to vote\nHigher is more value")
ax2.set_xticks(np.arange(len(score)))
ax2.set_xticklabels(countries)
ax2.tick_params(axis = "x", rotation = 90)
rects2 = ax2.bar(np.arange(len(score)), score, 0.5)

plt.show("Out")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
