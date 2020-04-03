---
title: matplotlib example fenics-basic-import (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'fenics-basic-import'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import mshr`
* `import dolfin`

## python fenics-basic-import

Python matplotlib example: fenics-basic-import

```python
import dolfin
import mshr
import numpy as np
import matplotlib.pyplot as plt
dolfin.parameters["form_compiler"]["cpp_optimize"] = True
dolfin.parameters["form_compiler"]["representation"] = "uflacs"
plt.style.use('seaborn-notebook')
%matplotlib inline


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
