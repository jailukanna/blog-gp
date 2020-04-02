---
title: PIL example about pil mnist (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'about pil mnist'

Functions in program: 
* `def save_images_from_dataframes(folder="/media/alex/Data/MNIST/") :`
* `def about_pil() : `

Modules used in program: 
* `import seaborn as sns`
* `import pandas as pd`
* `import numpy as np `
* `import os, sys, logging`

## python about pil mnist

Python PIL example: about pil mnist

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-   

# builtin
import os, sys, logging
from pprint(import pprint)
from collections import Iterable

# data
import numpy as np 
import pandas as pd

# visualization
from matplotlib import pyplot as plt
import seaborn as sns
from PIL import Image

# logging
l = logging.INFO
logging.basicConfig(level=l, format="%(levelname)s : %(message)s")
info = logging.info

# filepaths
FOLDER  = "/home/alex/Bureau/MNIST/"
# my_dir  = '/home/alex/Bureau/'
print(os.listdir(FOLDER))

TRAIN = "train.csv"
TEST  = 'test.csv'


# init 
train = pd.read_csv(FOLDER+TRAIN)
sub   = pd.read_csv(FOLDER+TEST)
train.head()

# control
print(type(train))
print(train.shape)


# feature, target split 
X_train, y_train = np.array(train.drop("label", axis=1)), train.label.values
X_sub            = np.array(sub)






def about_pil() : 

    # from array to img
    X_train, y_train = np.array(train.drop("label", axis=1)), train.label.values
    img = X_train[0]
    img2 = np.array([np.array([i, i, i, 255]) for i in img])
    img3 = img2.reshape((28, 28, 4))
    img3 = img3.astype(np.uint8)
    img4 = Image.fromarray(img3)
    img4.save("/home/alex/Bureau/pil.png")

    # from img to arr
    img5 = Image.open("/home/alex/Bureau/my_1.png")
    arr5 = np.array(img5)


def save_images_from_dataframes(folder="/media/alex/Data/MNIST/") :

    # build dir if needed
    train_dir = folder + "train/"
    sub_dir   = folder + "sub/"
    if not os.path.isdir(train_dir) : os.mkdir(train_dir)
    if not os.path.isdir(sub_dir)   : os.mkdir(sub_dir)
 
    # create train sub folders
    sub_folder_dict = dict()
    for i in range(10) : 
        sub_folder = train_dir+str(i)+"/"
        if not os.path.isdir(sub_folder) : os.mkdir(sub_folder)
        sub_folder_dict[i] = sub_folder

    # control
    assert isinstance(train, pd.DataFrame)
    assert train.shape == (42000, 785)
    
    # handle train
    l = len(train)
    X_train, y_train = np.array(train.drop("label", axis=1)), train.label.values
    for i in train.index : 
        
        x_i, y_i = X_train[i], int(y_train[i])
        filename = sub_folder_dict[y_i] + str(i) + ".png"

        if os.path.isfile(filename) : 
            info("already created")
            continue

        # assert isinstance(x_i, np.ndarray)
        # assert x_i.shape[0] == 784
        # assert isinstance(y_i, int) 

        img2 = np.array([np.array([i, i, i, 255]) for i in x_i])
        img3 = img2.reshape((28, 28, 4))
        img3 = img3.astype(np.uint8)
        img4 = Image.fromarray(img3)
        img4.save(filename)

        txt = "{} sur {}".format(i, l)
        info(txt)


    # handle sub
    l = len(sub)
    for i in sub.index : 
        
        x_i = sub.loc[i]
        filename = sub_dir + str(i) + ".png"

        if os.path.isfile(filename) : 
            info("already created")
            continue

        # assert isinstance(x_i, np.ndarray)
        # assert x_i.shape[0] == 784
        # assert isinstance(y_i, int) 

        img2 = np.array([np.array([i, i, i, 255]) for i in x_i])
        img3 = img2.reshape((28, 28, 4))
        img3 = img3.astype(np.uint8)
        img4 = Image.fromarray(img3)
        img4.save(filename)

        txt = "{} sur {}".format(i, l)
        info(txt)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
