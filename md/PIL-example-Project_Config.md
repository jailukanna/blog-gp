---
title: PIL example Project Config (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Project Config'


Modules used in program: 
* `import os`

## python Project Config

Python PIL example: Project Config

```python
import os


DATA_DIR = "/home/jasper/Documents/BP_Jasp/data/pg"
IMAGE_DIR = os.path.join(DATA_DIR, "123-130/")
#DATA_DIR = '/home/jasper/Documents/BP_Jasp/data/pg/valSet/sedimentation/'
#IMAGE_DIR = os.path.join(DATA_DIR, "T0132_S002_U010/")
model_dir = '/home/jasper/Documents/BP_Jasp/project/nets/conv_autoencoder'
GrayScale = True
LOG_DIR = '/home/jasper/Documents/BP_Jasp/logs/Siam/'
train_batch_size = 32
train_number_epochs = 50

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
