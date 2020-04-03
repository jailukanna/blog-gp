---
title: matplotlib example pandasNumpy (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'pandasNumpy'


Modules used in program: 
* `import os`
* `import pandas as pd`
* `import numpy as np`

## python pandasNumpy

Python matplotlib example: pandasNumpy

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import numpy as np
import pandas as pd
import os

os.system('cls')

#Numpy
arreglo = np.zeros((3,4))
print("Arreglo de 3 x 4 (llenado con ceros):\n",arreglo)
arreglo = np.ones((2,3,4),dtype=np.int16)
print("Arreglo de 2 X 3 X 4 (llenado con unos):\n",arreglo)

#Pandas
datos_csv = pd.read_csv('datos.csv')
print("Contenido del CSV:\n",datos_csv)
print("Contenido (datos_csv[:0]) :\n",datos_csv[:0])
print("Contenido (datos_csv[:1]) :\n",datos_csv[:1])
print("Contenido (datos_csv[:2]) :\n",datos_csv[:2])
print("Contenido (datos_csv[:3]) :\n",datos_csv[:3])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
