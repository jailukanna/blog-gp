---
title: pil example Project dataReader DataReader (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Project dataReader DataReader'

Functions in program: 
* `def write_list(triples, dir):`
* `def load_checkpoint(model,optimize = None ):`
* `def save_checkpoint(net, epoch,optimizer):`
* `def get_triples(image_dir):`
* `def train_test_split(triples, splits):`

Modules used in program: 
* `import torch`
* `import shutil`
* `import matplotlib.pyplot as plt`
* `import os`
* `import numpy as np`
* `import itertools`

## python Project dataReader DataReader

Python pil example: Project dataReader DataReader

```python
from __future__ import division, print_function
import itertools
import numpy as np
import os
import matplotlib.pyplot as plt
import shutil
import torch

from keras.preprocessing.image import ImageDataGenerator
from keras.utils import np_utils
from scipy.misc import imresize

from util.config import model_dir

epoch = 0



def train_test_split(triples, splits):
    lengt = len(triples)
    train = round(splits[0]*lengt)
    test = round(splits[0]+ splits[1] * lengt)
    val = round(splits[0]+splits[1]+splits[2] * lengt)
    return int(train),int(test),int(val)


def get_triples(image_dir):
    image_groups = {}
    for image_name in os.listdir(image_dir):
        base_name = image_name[0:-4]
        group_name = base_name[0:4]
        if image_groups.has_key(group_name):
            image_groups[group_name].append(image_name)
        else:
            image_groups[group_name] = [image_name]
    num_sims = 0
    image_triples = []
    group_list = sorted(list(image_groups.keys()))
    for i, g in enumerate(group_list):
        if num_sims % 100 == 0:
            print("Generated {:d} pos + {:d} neg = {:d} total image triples"
                  .format(num_sims, num_sims, 2 * num_sims))
        images_in_group = image_groups[g]
        sim_pairs_it = itertools.combinations(images_in_group, 2)
        # for each similar pair, generate a corresponding different pair
        for ref_image, sim_image in sim_pairs_it:
            image_triples.append((ref_image,sim_image,1))#(ref_image, sim_image, 1),image_dir)
            num_sims += 1
            #while True:
            #    j = np.random.randint(low=0, high=len(group_list), size=1)[0]
            #    if j != i:
            #        break
            #dif_image_candidates = image_groups[group_list[j]]
            #k = np.random.randint(low=0, high=len(dif_image_candidates), size=1)[0]
            #dif_image = dif_image_candidates[k]
            #image_triples.append((ref_image, dif_image, 0))
    print("Generated {:d} pos + {:d} neg = {:d} total image triples"
          .format(num_sims, num_sims, 2 * num_sims))
    return image_triples

def save_checkpoint(net, epoch,optimizer):
    torch.save({
        'epoch': epoch + 1,
        'state_dict': net.state_dict(),
        'optimizer': optimizer.state_dict(),
    },model_dir  + '2.pth.tar')


def load_checkpoint(model,optimize = None ):
    checkpoint = torch.load(model_dir  + '2.pth.tar')
    model_state = model.state_dict()
    new_params = model.state_dict()
    new_params.update(checkpoint)
    model.load_state_dict(checkpoint['state_dict'])
    if optimize != None:
        optimize.load_state_dict(checkpoint['optimizer'])
        epoch = checkpoint['epoch']
    return model

def write_list(triples, dir):
        with open(dir + ".csv", "w+") as my_csv:
            csvWriter = csv.writer(my_csv, delimiter=',')
            csvWriter.writerows(triples)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
