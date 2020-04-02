---
title: PIL example deeplab demo (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'deeplab demo'

Functions in program: 
* `def run_demo_image(image_name):`
* `def vis_segmentation(image, seg_map):`

Modules used in program: 
* `import get_dataset_colormap`
* `import tensorflow as tf`
* `import cv2`
* `import numpy as np`
* `import urllib`
* `import tempfile`
* `import tarfile`
* `import sys`
* `import StringIO`
* `import os`
* `import collections`

## python deeplab demo

Python PIL example: deeplab demo

```python

# coding: utf-8

# # DeepLab Demo
# 
# This demo will demostrate the steps to run deeplab semantic segmentation model on sample input images.
# 
# ## Prerequisites
# 
# Running this demo requires the following libraries:
# 
# * Jupyter notebook (Python 2)
# * Tensorflow (>= v1.5.0)
# * Matplotlib
# * Pillow
# * numpy
# * ipywidgets (follow the setup [here](https://ipywidgets.readthedocs.io/en/stable/user_install.html))

# ## Imports

# In[1]:


import collections
import os
import StringIO
import sys
import tarfile
import tempfile
import urllib

from IPython import display
from ipywidgets import interact
from ipywidgets import interactive
from matplotlib import gridspec
from matplotlib import pyplot as plt
import numpy as np
import cv2
from PIL import Image
from PIL import ImageFile
ImageFile.LOAD_TRUNCATED_IMAGES = True

import tensorflow as tf

print('\nprogram started!')

if tf.__version__ < '1.5.0':
    raise ImportError('Please upgrade your tensorflow installation to v1.5.0 or newer!')

# Needed to show segmentation colormap labels
sys.path.append('/home/suganuma/of_v0.9.8_linux64_release/apps/vscode_oF/semantic-segmentation/bin/data/python/utils')
import get_dataset_colormap

class DeepLabModel():
    """Class to load deeplab model and run inference."""
    
    INPUT_TENSOR_NAME = 'ImageTensor:0'
    OUTPUT_TENSOR_NAME = 'SemanticPredictions:0'
    INPUT_SIZE = 513

    def __init__(self):
        print('Init DeepLabModel()')
        """Creates and loads pretrained deeplab model."""
        self.graph = tf.Graph()
        
        graph_def = None

        # open model file
        print('\nload the model!')
        with open('/home/suganuma/tensorflow/models/research/deeplab/datasets/pascal_voc_seg/init_models/deeplabv3_pascal_train_aug/frozen_inference_graph.pb', 'r') as f:
            graph_def = tf.GraphDef.FromString(f.read())
        f.closed
        print('\nFished to load the model!')
        
        if graph_def is None:
            raise RuntimeError('Cannot find inference graph in tar archive.')

        with self.graph.as_default():      
            tf.import_graph_def(graph_def, name='')
        
        self.sess = tf.Session(graph=self.graph)
            
    def run(self, image):
        """Runs inference on a single image.
        
        Args:
            image: A PIL.Image object, raw input image.
            
        Returns:
            resized_image: RGB image resized from original input image.
            seg_map: Segmentation map of `resized_image`.
        """

        """
        * seg_mapというやつが、リサイズされた画像のsegmentation mapっぽい

        """

        width, height = image.size
        resize_ratio = 1.0 * self.INPUT_SIZE / max(width, height)
        target_size = (int(resize_ratio * width), int(resize_ratio * height))
        resized_image = image.convert('RGB').resize(target_size, Image.ANTIALIAS)
        # self.sess.run→tf.session.runでこの関数を呼んでるっぽい？
        batch_seg_map = self.sess.run(
            self.OUTPUT_TENSOR_NAME,
            feed_dict={self.INPUT_TENSOR_NAME: [np.asarray(resized_image)]})
        seg_map = batch_seg_map[0]
        return resized_image, seg_map


# In[5]:


# model = DeepLabModel(download_path)
model = DeepLabModel()


# ## Helper methods

# In[6]:


LABEL_NAMES = np.asarray([
    'background', 'aeroplane', 'bicycle', 'bird', 'boat', 'bottle',
    'bus', 'car', 'cat', 'chair', 'cow', 'diningtable', 'dog',
    'horse', 'motorbike', 'person', 'pottedplant', 'sheep', 'sofa',
    'train', 'tv'
])

FULL_LABEL_MAP = np.arange(len(LABEL_NAMES)).reshape(len(LABEL_NAMES), 1)
# ラベル名と色がもともとひもづけられていて、それのマッピングてきな？
FULL_COLOR_MAP = get_dataset_colormap.label_to_color_image(FULL_LABEL_MAP)

# visualize segmentaionな気がする
def vis_segmentation(image, seg_map):
    seg_image = get_dataset_colormap.label_to_color_image(
        seg_map, get_dataset_colormap.get_pascal_name()).astype(np.uint8)
    #sgnm
    #segmentaion color mapからnumpyでarray取得して、その配列からimage生成してるみたいな
    # seg_imageを配列化している。
    im = np.array(seg_image)
    pil_img = Image.fromarray(im)
    pil_img.save("/home/suganuma/Desktop/seg_image.jpg")
    # print("========================")
    # print(im)
    imgray = cv2.cvtColor(im,cv2.COLOR_BGR2GRAY)  # グレースケール画像に変換
    # ret,thresh = cv2.threshold(imgray,127,255,0)  # 127/255で二値化
    image, contours, hierarchy = cv2.findContours(imgray,cv2.RETR_TREE,cv2.CHAIN_APPROX_SIMPLE)  # 輪郭を検出    
    # print("========================")
    # print("contours: ")
    # print(contours[0])
    contour_image = Image.fromarray(image)
    contour_image.save('/home/suganuma/Desktop/output.jpg')
    # pil_img.show()
    print('\nsaved image!')
    #sgnm

# ## Run on sample images

# In[7]:


# Note that we are using single scale inference in the demo for fast
# computation, so the results may slightly differ from the visualizations
# in README, which uses multi-scale and left-right flipped inputs.

IMAGE_DIR = 'g3doc/img'

def run_demo_image(image_name):
    print("image name: " + image_name)
    try:
        image_path = os.path.join(IMAGE_DIR, image_name)
        orignal_im = Image.open(image_path)
    except IOError:
        print('Failed to read image from %s.' % image_path )
        return 
    print('\nrunning deeplab on image %s...' % image_name)
    # modelはDeepLabModelのインスタンス、それのrun
    resized_im, seg_map = model.run(orignal_im)
    
    vis_segmentation(resized_im, seg_map)

# TODO: I guess here!!!
# _ = interact(run_demo_image, image_name=['image1.jpg', 'image2.jpg', 'image3.jpg'])
# run_demo_image('image1.jpg')

# while loop!!!
# file_path = "/home/suganuma/of_v0.9.8_linux64_release/apps/vscode_oF/semantic-segmentation/bin/data/input.jpg"
# while(os.path.isfile(file_path)):
#     print(os.path.isfile(file_path))
#     run_demo_image(file_path)
# else:
#     print("File not found!!")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
