---
title: PIL example Project dataReader Relabeler (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Project dataReader Relabeler'

Functions in program: 
* `def relabel_failure(image_dir):`
* `def label_set_single(triple, IMAGE_DIR):`
* `def _correct():`
* `def _yes(event, obj, val , failure):`
* `def copy_image(src,des):`
* `def relabel(image_dir):`
* `def label_set_single(triple, IMAGE_DIR):`
* `def _yes(event, obj, val):`
* `def relabel_dir(image_dir):`

Modules used in program: 
* `import shutil`
* `import matplotlib.pyplot as plt`
* `import os`
* `import numpy as np`
* `import itertools`
* `import glob`
* `import fnmatch`
* `import csv`

## python Project dataReader Relabeler

Python PIL example: Project dataReader Relabeler

```python
from __future__ import division, print_function

import csv
import fnmatch
import glob
import itertools
import numpy as np
import os
import matplotlib.pyplot as plt
import shutil

from keras.preprocessing.image import ImageDataGenerator
from keras.utils import np_utils
from matplotlib.widgets import Button, RadioButtons
from scipy.misc import imresize

from DataReader import get_triples
from util.colorConstancy import max_white, retinex_adjust, retinex_with_adjust
#from util.config import IMAGE_DIR
stable= False
DATA_DIR = '/home/jasper/Documents/BP_Jasp/data/pg/valSet/splitting/'
IMAGE_DIR = os.path.join(DATA_DIR, "T0131_S000_U010/")
triple_list = []






def relabel_dir(image_dir):
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
        images_in_group = image_groups[g]
        sim_pairs_it = itertools.combinations(images_in_group, 2)
        for ref_image, sim_image in sim_pairs_it:
            image_triples.append((ref_image,sim_image,1))
        print(num_sims)
        return image_triples

def _yes(event, obj, val):
        tuple = [obj[0], obj[1], val]
        triple_list.append(tuple)
        plt.close(1)
        pass

def label_set_single(triple, IMAGE_DIR):
        print(triple)
        ref_image = plt.imread(os.path.join(IMAGE_DIR, triple[0]))
        sim_image = plt.imread(os.path.join(IMAGE_DIR, triple[1]))
        similar = "similar"
        if (triple[2] == 0):
            similar = "not similar"
        plt.figure(1)
        plt.subplot(131)
        plt.imshow(ref_image)
        plt.subplot(132)
        plt.imshow(sim_image)
        axcut = plt.axes([0.9, 0.0, 0.1, 0.075])
        sim = Button(axcut, 'YES', color='green', hovercolor='green')
        sim.on_clicked(lambda x: _yes(x, triple, 1))
        axNotSim = plt.axes([0.7, 0.0, 0.1, 0.075])
        notSim = Button(axNotSim, 'no', color='red', hovercolor='red')
        notSim.on_clicked(lambda x: _yes(x, triple, 0))
        figManager = plt.get_current_fig_manager()
        figManager.window.showMaximized()
        plt.title(similar)
        plt.tight_layout()
        plt.show()
        return triple

def relabel(image_dir):
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
            triple = label_set_single((ref_image,sim_image,1),IMAGE_DIR)

            image_triples.append(triple)#(ref_image, sim_image, 1),image_dir)
            num_sims += 1
    return image_triples





def copy_image(src,des):
    shutil.copy(src, des)


failure_pairs = []
image_triples = []
def _yes(event, obj, val , failure):
    tuple = [obj[0], obj[1], val]
    image_triples.append(tuple)
    failure_pairs.append([obj[1],failure])
    print((tuple))
    print((failure))
    plt.close(1)
    pass

def _correct():
    triple = image_triples[-1]
    if triple[2] == 1:
        triple[2]=0
    else:
        triple[2]=1
    print((image_triples[-1]))

def label_set_single(triple, IMAGE_DIR):
    ref_image = plt.imread(os.path.join(IMAGE_DIR, triple[0]))
    sim_image = plt.imread(os.path.join(IMAGE_DIR, triple[1]))
    similar = "similar"
    if (triple[2] == 0):
        similar = "not similar"
    plt.figure(1)
    plt.subplot(131)
    plt.imshow(max_white(ref_image))
    plt.subplot(132)
    plt.imshow(max_white(sim_image))
    plt.subplot(133)
    plt.imshow(ref_image - sim_image)
    rax = plt.axes([0.3, 0.0, 0.1, 0.075], facecolor='lightgoldenrodyellow')
    radio3 = RadioButtons(rax, ('stable','splitting' ,'creaming','stable',  'colorChange'))
    axcut = plt.axes([0.9, 0.0, 0.1, 0.075])
    sim = Button(axcut, 'YES', color='green', hovercolor='green')
    sim.on_clicked(lambda x: _yes(x, triple, 1,radio3.value_selected))
    axNotSim = plt.axes([0.7, 0.0, 0.1, 0.075])
    notSim = Button(axNotSim, 'no', color='red', hovercolor='red')
    notSim.on_clicked(lambda x: _yes(x, triple, 0,radio3.value_selected))
    axNotSim = plt.axes([0.5, 0.0, 0.1, 0.075])
    corr = Button(axNotSim, 'correct previous', color='blue', hovercolor='blue')
    corr.on_clicked(lambda x: _correct())
    figManager = plt.get_current_fig_manager()
    figManager.window.showMaximized()
    plt.title(similar)
    plt.tight_layout()
    plt.show()
    return triple


def relabel_failure(image_dir):
    image_groups = {}
    for image_name in os.listdir(image_dir):
        group_name,base_name  = image_name.split("-")
        if image_groups.has_key(group_name):
            image_groups[group_name].append(image_name)
        else:
            image_groups[group_name] = [image_name]
    group_list = sorted(list(image_groups.keys()))
    for i, g in enumerate(group_list):
        images_in_group = image_groups[g]
        sim_pairs_it = itertools.combinations(images_in_group, 2)
        ref_img = group_name+"-18.jpg"
        interval = int(len(images_in_group)/100)

        for i in range(0,len(images_in_group),interval):
            sim_image = images_in_group[i]
            copy_image(os.path.join(IMAGE_DIR, sim_image), '/home/jasper/Documents/BP_Jasp/data/pg/100per')
            print((i))
            if stable:
                image_triples.append((ref_img, sim_image, 1))
            else:
                label_set_single((ref_img, sim_image, 1), IMAGE_DIR)
    return image_triples

#for j in range(0,1):
#    for i in range(0 , 10):
#        IMAGE_DIR = os.path.join(DATA_DIRr, "T0132_S0"+str(j)+str(i) +"_U010/")
#        print(IMAGE_DIR)
#triples = relabel_failure(IMAGE_DIR)


#write_list(image_triples, IMAGE_DIR+'changesTripples')
#write_list(failure_pairs, IMAGE_DIR+'failure')




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
