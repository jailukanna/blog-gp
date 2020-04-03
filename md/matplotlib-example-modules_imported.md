---
title: matplotlib example modules imported (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'modules imported'


Modules used in program: 
* `import warnings; warnings.simplefilter('ignore')  # Fix NumPy issues.`
* `import urllib.request, urllib.parse, urllib.error`
* `import time`
* `import tensorflow as tf`
* `import sys`
* `import string`
* `import statsmodels.api as sm`
* `import ssl`
* `import sqlite3`
* `import SocketServer`
* `import skimage.data`
* `import shutil`
* `import seaborn as sns; sns.set()`
* `import seaborn as sns`
* `import scipy.cluster.hierarchy as sch`
* `import re`
* `import random`
* `import os`
* `import numexpr`
* `import nbformat`
* `import matplotlib_venn as venn`
* `import matplotlib.pyplot as plt`
* `import matplotlib as mpl`
* `import math`
* `import keras.optimizers as opti`
* `import keras`
* `import itertools`
* `import ipywidgets as widgets`
* `import IPython.display as disp`
* `import helpers_05_08`
* `import fabric.contrib.project as project`
* `import csv`
* `import collections`
* `import array`

## python modules imported

Python matplotlib example: modules imported

```python
# curl -si https://api.github.com/users/tirthajyoti/repos | grep url | cut -d '"' -f4 | xargs -i git clone {} 

# grep -wR import * | awk -F":" '{print($2}' | sort -u)

from skimage import data, color, feature
from skimage import data, transform
from sklearn.base import BaseEstimator, ClassifierMixin
from sklearn.base import BaseEstimator, TransformerMixin
from sklearn.cluster import AffinityPropagation
from sklearn.cluster import AgglomerativeClustering
from sklearn.cluster import KMeans
from sklearn.cluster import MeanShift
from sklearn.cluster import MiniBatchKMeans
from sklearn.cluster import SpectralClustering
from sklearn.cross_validation import cross_val_score
from sklearn.cross_validation import LeaveOneOut
from sklearn.cross_validation import train_test_split
from sklearn.datasets import fetch_20newsgroups
from sklearn.datasets import fetch_lfw_people
from sklearn.datasets import fetch_mldata
from sklearn.datasets import fetch_olivetti_faces
from sklearn.datasets import fetch_species_distributions
from sklearn.datasets import load_breast_cancer
from sklearn.datasets import load_digits
from sklearn.datasets import load_iris
from sklearn.datasets import load_sample_image
from sklearn.datasets import make_blobs
from sklearn.datasets import make_classification
from sklearn.datasets import make_moons
from sklearn.datasets import make_regression
from sklearn.datasets import make_swiss_roll
from sklearn.datasets.samples_generator import make_blobs
from sklearn.datasets.samples_generator import make_circles
from sklearn.datasets.species_distributions import construct_grids
from sklearn.decomposition import PCA
from sklearn.decomposition import RandomizedPCA
from sklearn.ensemble import AdaBoostRegressor
from sklearn.ensemble import BaggingClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import RandomForestRegressor
from sklearn.feature_extraction.image import PatchExtractor
from sklearn.feature_extraction import DictVectorizer
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.gaussian_process import GaussianProcess
from sklearn.grid_search import GridSearchCV
from sklearn import cluster, datasets
from sklearn import metrics
from sklearn import preprocessing
from sklearn import tree
from sklearn.learning_curve import learning_curve
from sklearn.learning_curve import validation_curve
from sklearn.linear_model import LassoCV
from sklearn.linear_model import Lasso
from sklearn.linear_model import LinearRegression
from sklearn.linear_model import logistic
from sklearn.linear_model import LogisticRegressionCV
from sklearn.linear_model import LogisticRegression
from sklearn.linear_model import RidgeCV
from sklearn.linear_model import Ridge
from sklearn.manifold import Isomap
from sklearn.manifold import LocallyLinearEmbedding
from sklearn.manifold import MDS
from sklearn.manifold import TSNE
from sklearn.metrics import accuracy_score
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import f1_score
from sklearn.metrics import pairwise_distances_argmin
from sklearn.metrics import pairwise_distances
from sklearn.metrics import precision_score
from sklearn.metrics import recall_score
from sklearn.mixture import GMM  
from sklearn.model_selection import GridSearchCV
from sklearn.model_selection import StratifiedKFold
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import GaussianNB
from sklearn.naive_bayes import MultinomialNB
from sklearn.neighbors import KernelDensity
from sklearn.neighbors import KNeighborsClassifier
from sklearn.neighbors import NearestNeighbors
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import Imputer
from sklearn.preprocessing import LabelEncoder
from sklearn.preprocessing import MinMaxScaler
from sklearn.preprocessing import OneHotEncoder
from sklearn.preprocessing import PolynomialFeatures
from sklearn.preprocessing import StandardScaler
from sklearn.svm import LinearSVC
from sklearn.svm import SVC
from sklearn.svm import SVR
from sklearn.tree import DecisionTreeClassifier
from sklearn.tree import DecisionTreeRegressor
from sklearn.utils import resample
from sklearn.utils.validation import column_or_1d


from datetime import datetime
from dateutil import parser
from diversipy import *
from DOE_functions import *
from fabric.api import *
from fakedbclass import fakedb
from faker import Faker
from __future__ import unicode_literals
from generate_contents import iter_notebooks, NOTEBOOK_DIR
from generate_contents import NOTEBOOK_DIR, REG, iter_notebooks, get_notebook_title
from Generate_DOE import *
from helpers_05_08 import visualize_tree
from ipykernel import kernelspec as ks
from IPython.display import display 
from IPython.display import Image
from ipywidgets import HBox, Label, FloatSlider

from ipywidgets import interact, interactive, fixed, interact_manual
from ipywidgets import interact, interactive, IntSlider, Layout, interact_manual

from itertools import chain
from itertools import product as prod
from keras.callbacks import ModelCheckpoint   
from keras.datasets import cifar10
from keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Dropout
from keras.layers import Dense, Activation,Dropout
from keras.models import Sequential
from keras.utils import np_utils
from math import factorial
from math import log10 as lg10
from matplotlib.collections import LineCollection
from matplotlib.colors import LinearSegmentedColormap
from matplotlib.image import imread
from matplotlib import cm as cm
from matplotlib import cycler
from matplotlib import gridspec
from matplotlib import offsetbox
from matplotlib import pyplot as plt
from matplotlib.legend import Legend
from matplotlib.patches import Ellipse
from matplotlib.tri import Triangulation
from mpl_toolkits.basemap import Basemap

from mpl_toolkits import mplot3d

from mpl_toolkits.mplot3d.art3d import Line3DCollection
from mprun_demo import sum_of_lists
from nbformat.v4.nbbase import new_markdown_cell
from netCDF4 import Dataset
from netCDF4 import date2index
from numpy import nan
from numpy.random import randint as ri
from numpy.random import randn as rn
from os import path
from pandas.core import datetools
from pandas_datareader import data
from pandas.tseries.holiday import USFederalHolidayCalendar
from pandas.tseries.offsets import BDay
from pelicanconf import *
from pelican.server import ComplexHTTPRequestHandler
from pydbgen import pydb
from pyDOE_corrected import *
from pyDOE.doe_factorial import ff2n
from pyDOE.doe_repeat_center import repeat_center
from pyDOE import *
from pylab import rcParams
from random import randint,choice

from Read_Write_CSV import *

from scipy import linspace, polyval, polyfit, sqrt, stats, randn, optimize
from scipy import special
from scipy import stats
from scipy.optimize import curve_fit as cf
from scipy.spatial.distance import cdist
from scipy.special import binom  
from scipy.stats import bernoulli
from scipy.stats import beta
from scipy.stats import binom
from scipy.stats import chi2,ncx2
from scipy.stats import f,ncf
from scipy.stats import gamma
from scipy.stats import gaussian_kde
from scipy.stats import geom
from scipy.stats import mode
from scipy.stats import norm
from scipy.stats import poisson
from scipy.stats import t,nct
from scipy.stats import uniform
from setuptools import setup
from six import moves
from six.moves.urllib.request import urlopen

from sympy import init_printing
from sympy import latex
from sympy import *
from sympy import Symbol,sympify

from tensorflow.examples.tutorials.mnist import input_data
from tensorflow import logging
from tqdm import tqdm 
from UCI_ML_Functions import *
from User_input import *
import array
import collections
import csv
import fabric.contrib.project as project
import helpers_05_08
import IPython.display as disp
import ipywidgets as widgets
import itertools
import keras
import keras.optimizers as opti
import math
import matplotlib as mpl
import matplotlib.pyplot as plt
import matplotlib_venn as venn
import nbformat
import numexpr
import os
import random
import re
import scipy.cluster.hierarchy as sch
import seaborn as sns
import seaborn as sns; sns.set()


import shutil
import skimage.data
import SocketServer
import sqlite3

import ssl

import statsmodels.api as sm

import string
import sys
import tensorflow as tf

import time

import urllib.request, urllib.parse, urllib.error
import warnings; warnings.simplefilter('ignore')  # Fix NumPy issues.


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
