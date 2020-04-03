---
title: matplotlib example train (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'train'


Modules used in program: 
* `import sys`
* `import copy`
* `import os`
* `import cv2`
* `import pickle`
* `import random`
* `import argparse`
* `import numpy as np`
* `import matplotlib.image as mpimg`
* `import matplotlib.pyplot as plt`

## python train

Python matplotlib example: train

```python
# import the VGG model from the module vgg.py
from vgg import *

# import the necessary packages
from sklearn.preprocessing import LabelBinarizer
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report
from keras.preprocessing.image import ImageDataGenerator
from keras.optimizers import SGD, Adam, Adagrad
from keras.callbacks import ModelCheckpoint, TerminateOnNaN, TensorBoard,ReduceLROnPlateau
from imutils import paths
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
import numpy as np
import argparse
import random
import pickle
import cv2
import os
import copy
import sys
from loguru import logger

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
