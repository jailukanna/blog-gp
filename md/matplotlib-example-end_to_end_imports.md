---
title: matplotlib example end to end imports (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'end to end imports'


Modules used in program: 
* `import matplotlib.image as mpimg`
* `import matplotlib.pyplot as plt`
* `import cv2`
* `import keras`
* `import tensorflow as tf`
* `import pandas as pd`
* `import numpy as np`
* `import pickle`
* `import datetime`
* `import fnmatch`
* `import random`
* `import os`

## python end to end imports

Python matplotlib example: end to end imports

```python
# python standard libraries
import os
import random
import fnmatch
import datetime
import pickle

# data processing
import numpy as np
np.set_printoptions(formatter={'float_kind':lambda x: "%.4f" % x})

import pandas as pd
pd.set_option('display.width', 300)
pd.set_option('display.float_format', '{:,.4f}'.format)
pd.set_option('display.max_colwidth', 200)

# tensorflow
import tensorflow as tf
import keras
from keras.models import Sequential  # V2 is tensorflow.keras.xxxx, V1 is keras.xxx
from keras.layers import Conv2D, MaxPool2D, Dropout, Flatten, Dense
from keras.optimizers import Adam
from keras.models import load_model
print( f'tf.__version__: {tf.__version__}' )
print( f'keras.__version__: {keras.__version__}' )

# sklearn
from sklearn.utils import shuffle
from sklearn.model_selection import train_test_split

# imaging
import cv2
from imgaug import augmenters as img_aug
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
%matplotlib inline
from PIL import Image

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
