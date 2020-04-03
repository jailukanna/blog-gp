---
title: matplotlib example import path (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'import path'


Modules used in program: 
* `import keras`
* `import os`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python import path

Python matplotlib example: import path

```python
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
import os
from pathlib import Path

import keras
from keras.preprocessing.image import ImageDataGenerator
from keras.applications import ResNet50
from keras.applications.resnet50 import preprocess_input
from keras import Model, layers

base_dir = Path("../../../hymenoptera_data/")
train_dir = os.path.join(base_dir, 'train')
validation_dir = os.path.join(base_dir, 'val')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
