---
title: matplotlib example Import Modules (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'Import Modules'


Modules used in program: 
* `import pandas as pd`
* `import pickle`
* `import numpy as np`
* `import math`
* `import matplotlib.pyplot as plt`
* `import matplotlib.gridspec as gridspec`
* `import cv2`

## python Import Modules

Python matplotlib example: Import Modules

```python
#######skimage for Image Transformations
from skimage import img_as_float
from skimage import img_as_ubyte
from skimage import exposure
from skimage import filters
from skimage import color
from skimage.transform import rotate
from skimage.transform import warp
from skimage.transform import ProjectiveTransform
import cv2

######## Matplotlib for Displaying Plots
import matplotlib.gridspec as gridspec
import matplotlib.pyplot as plt

######## Random and Math and Numpy for Mathematical Operations
from random import randint
from random import uniform
import math
from numpy import sqrt
import numpy as np
from numpy import linalg
from numpy import dot


#####Pickle for Caching, Storing and Retrieving Data
import pickle

###### Import Shuffling function from SKLEARN
from sklearn.utils import shuffle
from sklearn.model_selection import train_test_split

####### Pandas For Data Visualization, TQDM for Progress Bar
import pandas as pd
from tqdm import trange
from IPython.display import display, HTML
%matplotlib inline

print("Modules Imported")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
