---
title: matplotlib example grafv (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'grafv'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib.mlab as mlab`
* `import numpy as np`
* `import sys`

## python grafv

Python matplotlib example: grafv

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

'''
  Para geração de histograma das velocidades
'''

import sys
import numpy as np
import matplotlib.mlab as mlab
import matplotlib.pyplot as plt

# Tamanho da página do histograma
width = 20  # cm
height = 15 # cm

# Altura de queda
deltaS = 34 # metros

if len(sys.argv) < 3:
  print("Usage: generate.py data.txt output")
  exit(1)

output = sys.argv[2]

data = np.fromfile(sys.argv[1], sep='\n')

data = deltaS / data

num_bins = 12
avg = np.average(data)
std = np.std(data)

fig, ax = plt.subplots()
n, bins, patches = ax.hist(data, num_bins, color='burlywood', histtype='stepfilled')

print("Média: %s Desvio Padrão: %s" %(avg, std))

ax.axvline(avg, color='red', label="Média = %.2f m/s" %avg)
ax.axvline(avg-std, color='lightblue', label="Média - Desvio Padrão = %.2f m/s" %(avg-std))
ax.axvline(avg+std, color='darkblue', label="Média + Desvio Padrão = %.2f m/s" % (avg+std))

ax.set_xlabel('Velocidade (m/s)')
ax.set_ylabel('Contagem (n)')
ax.set_title('Histograma das Velocidades Médias')
ax.set_ylim( None, n.max() * 1.2)

legend = ax.legend(loc='upper left', shadow=True, prop={'size':10})
fig.tight_layout()

fig.set_size_inches(width / 2.54, height / 2.54)
plt.savefig('%s.png' %output, dpi=100)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
