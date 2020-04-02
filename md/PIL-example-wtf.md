---
title: PIL example wtf (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'wtf'

Functions in program: 
* `def listdir(folder):`
* `def openimage(image):`
* `def getimage(image):`
* `def getfile(data_key):`

Modules used in program: 
* `import boto3`
* `import numpy as np`
* `import keras`
* `import random`
* `import pandas as pd`
* `import os`
* `import numpy as np`
* `import PIL as pillow`

## python wtf

Python PIL example: wtf

```python
import PIL as pillow
from PIL import Image
import numpy as np
import os
import pandas as pd
import random


import keras
from matplotlib import pyplot as plt
import numpy as np
%matplotlib inline
from keras import backend as K
from keras.layers import Input,Conv2D,MaxPooling2D,UpSampling2D, Conv2DTranspose
from keras.models import Model
from keras.optimizers import RMSprop
from keras import callbacks

import boto3
from sagemaker import get_execution_role
from s3monkey import S3FS
from io import BytesIO

role = get_execution_role()
client = boto3.client('s3')
s3 = boto3.resource('s3')
bucket = s3.Bucket('beast-images')

def getfile(data_key):
    return f's3://beast-images/{data_key}'

def getimage(image):
    obj = client.get_object(Bucket='beast-images', Key=image)
    return BytesIO(obj['Body'].read())

def openimage(image):
    return Image.open(getimage(image))

def listdir(folder):
    return list(map(lambda x: x.key, bucket.objects.filter(Delimiter='/', Prefix=f'{folder}/')))[1:]
    
#openimage('A-Bathing-Ape-Bapesta-DC-Comics-Batman.png')
items = listdir('shoes')   

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
