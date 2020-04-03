---
title: matplotlib example super (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'super'

Functions in program: 
* `def GenSigmaChart(data, output, type, std):`
* `def GenHistogram(data, output, type, unit, title):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib.mlab as mlab`
* `import numpy as np`
* `import sys`

## python super

Python matplotlib example: super

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

import sys
import numpy as np
import matplotlib.mlab as mlab
import matplotlib.pyplot as plt


if len(sys.argv) < 4:
  print("Usage: generate.py data.txt type output")
  exit(1)

def GenHistogram(data, output, type, unit, title):
  width = 20  # cm
  height = 15 # cm
  num_bins = min(max(len(data) / 3, 2), 12)
  avg = np.average(data)
  std = np.std(data)

  fig, ax = plt.subplots()
  n, bins, patches = ax.hist(data, num_bins, color='burlywood', histtype='stepfilled')

  ax.axvline(avg, color='red', label="Média = %.2f %s" % (avg, unit))
  ax.axvline(avg-std, color='lightblue', label="Média - Desvio Padrão = %.2f %s" %(avg-std, unit))
  ax.axvline(avg+std, color='darkblue', label="Média + Desvio Padrão = %.2f %s" % (avg+std, unit))

  plt.text(0.025, 0.8, r'$\sigma = %.2f$' % std, transform = ax.transAxes)
  ax.set_xlabel('%s (%s)' %(type, unit))
  ax.set_ylabel('Contagem (n)')
  ax.set_title('%s' %title)
  ax.set_ylim( None, n.max() * 1.2)

  legend = ax.legend(loc='upper left', shadow=True, prop={'size':10})
  fig.tight_layout()

  fig.set_size_inches(width / 2.54, height / 2.54)
  plt.savefig('%s.png' %output, dpi=100)

def GenSigmaChart(data, output, type, std):
  #Tstd, "%s-sigma" %(output), 'Tempo de Queda'
  width = 20  # cm
  height = 15 # cm
  x = [2**i for i in range(len(data))]
  s = std / np.sqrt(x)
  fig, ax = plt.subplots()
  ax.plot(x, data, label="Calculado")
  ax.plot(x, s, label="Teórico")

  ax.set_xlabel('Número de Amostras usadas na média')
  ax.set_ylabel('Desvio Padrão')
  ax.set_title('Desvio Padrão da Média de %s' %type)

  legend = ax.legend(loc='upper right', shadow=True, prop={'size':10})
  fig.tight_layout()

  fig.set_size_inches(width / 2.54, height / 2.54)
  plt.savefig('%s.png' %output, dpi=100)

deltaS = 34 # metros

type = sys.argv[2].decode(encoding='UTF-8',errors='strict')
output = sys.argv[3]
data = np.fromfile(sys.argv[1], sep='\n')
dataV = deltaS / data
dataA = (2 * deltaS) / (data ** 2)

Tstd = []
Vstd = []
Astd = []

Tavg = []
Vavg = []
Aavg = []

for i in range(0,6):
  n = 2**i
  arrT = []
  arrV = []
  arrA = []

  print("Calculando para %s" %n)
  filenameT = "o/%s-%s-t.txt" %(output, n)
  filenameV = "o/%s-%s-v.txt" %(output, n)
  filenameA = "o/%s-%s-a.txt" %(output, n)

  print("Salvando em %s %s %s" % (filenameT, filenameV, filenameA))
  fT = open(filenameT, "w")
  fV = open(filenameV, "w")
  fA = open(filenameA, "w")

  for z in range(0, len(data), n):
    avgT = 0
    avgV = 0
    avgA = 0
    if z+n >= len(data):
      break;
    for k in range(0, n):
      avgT += data[z+k]
      avgV += dataV[z+k]
      avgA += dataA[z+k]

    avgT /= n
    avgV /= n
    avgA /= n

    arrT.append(avgT)
    arrV.append(avgV)
    arrA.append(avgA)

    fT.write("%.2f\n" %avgT)
    fV.write("%.2f\n" %avgV)
    fA.write("%.2f\n" %avgA)

  fT.close()
  fV.close()
  fA.close()

  npT = np.array(arrT)
  npV = np.array(arrV)
  npA = np.array(arrA)

  Tstd.append(np.std(npT))
  Vstd.append(np.std(npV))
  Astd.append(np.std(npA))

  print("Salvando Gráfico de Histograma do Tempo de Queda")
  GenHistogram(npT, "%s-%s-hist" %(output, n), 'Tempo de Queda', 's', 'Histograma de Tempo de Queda Média N=%s (%s)' %(n, type))

  Tavg.append(np.average(npT))
  Vavg.append(np.average(npV))
  Aavg.append(np.average(npA))


print("Desenhando gráfico sigma")

GenSigmaChart(Tstd, "%s-sigma" %(output), 'Tempo de Queda (%s)' %type, np.std(data))




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
