---
title: matplotlib example generate (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'generate'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib.mlab as mlab`
* `import numpy as np`
* `import sys`

## python generate

Python matplotlib example: generate

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

'''
  Para geração de histograma dos tempos de queda
'''

import sys
import numpy as np
import matplotlib.mlab as mlab
import matplotlib.pyplot as plt

# Tamanho da página do histograma
width = 20  # cm
height = 15 # cm

if len(sys.argv) < 4:
  print("Usage: generate.py data.txt title output")
  exit(1)

output = sys.argv[3]
title = sys.argv[2].decode(encoding='UTF-8',errors='strict')

data = np.fromfile(sys.argv[1], sep='\n')

num_bins = 12
avg = np.average(data)
std = np.std(data)

fig, ax = plt.subplots()
n, bins, patches = ax.hist(data, num_bins, color='burlywood', histtype='stepfilled')

print("Média: %s Desvio Padrão: %s" %(avg, std))

ax.axvline(avg, color='red', label="Média = %.2f s" %avg)
ax.axvline(avg-std, color='lightblue', label="Média - Desvio Padrão = %.2f s" %(avg-std))
ax.axvline(avg+std, color='darkblue', label="Média + Desvio Padrão = %.2f s" % (avg+std))

ax.set_xlabel('Tempo (s)')
ax.set_ylabel('Contagem (n)')
ax.set_title(title)
ax.set_ylim( None, n.max() * 1.2)

legend = ax.legend(loc='upper left', shadow=True, prop={'size':10})
fig.tight_layout()

fig.set_size_inches(width / 2.54, height / 2.54)
plt.savefig('%s.png' %output, dpi=100)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
