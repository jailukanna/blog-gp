---
title: mysql example Chapter7 8 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Chapter7 8'


Modules used in program: 
* `import numpy as np`
* `import pandas as pd`

## python Chapter7 8

Python mysql example: Chapter7 8

```python
import pandas as pd
import numpy as np
from Functions_Alejandro import contador_positivos, contador_positivosprinting, contador_negativos



a=pd.Series(np.random.randn(100))
print(a)
d=contador_negativos(a)
e=contador_positivos(a)

print('numeros negativos:',d,'\n','numeros positivos:',e)
if d>e:
    print('os numeros negativos sao mais do que os numeros positivos')
elif d<e:
    print('os numeros positivos sao mais do que os numeros negativos')
else:
    print('ha a mesma quantidade de numeros positivos e negativos')



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
