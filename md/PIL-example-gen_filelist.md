---
title: PIL example gen filelist (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'gen filelist'

Functions in program: 
* `def resize(im, id, path):`
* `def centerCrop(im, id):`

Modules used in program: 
* `import PIL`
* `import numpy as np`
* `import os`

## python gen filelist

Python PIL example: gen filelist

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
'''
    This simple script generates the high/low ranked image list from the AVA dataset.
    It also copies/resizes the source images to a specified folder.
    This script is assumed to be placed in AVA_ROOT with a folder name 'images', which contains all the source images.
    Make sure you alter the hard-coded path before using this script.
'''


import os
import numpy as np
import PIL
from PIL import Image

def centerCrop(im, id):
	left = (im.size[0] - 256) / 2
	top = (im.size[1] - 256) / 2
	im.crop((left, top, left+256, top+256)).save('/home/robin/Libraries/caffe/data/ava/images/'+str(id)+'.jpg')

def resize(im, id, path):
	baseLength = 500
	scale = 1
	if im.size[0] > im.size[1]:
		scale = float(baseLength) / im.size[0]
	else:
		scale = float(baseLength) / im.size[1]
	new_size = (int(im.size[0] * scale), int(im.size[1] * scale))
	im.resize(new_size, PIL.Image.ANTIALIAS).save(path+str(id)+'.jpg')



ava = np.loadtxt('AVA.txt', dtype=int)

ranking = ava[:,2:12]
#print(ranking)
weight = np.array(range(1,11), dtype=float)
ID = ava[:,1]
#print(ID)

total_score = np.dot(ranking, weight)
#print(total_score)
total_votes = np.sum(ranking, axis=1)
#print(total_votes)
avg_rank = np.divide(total_score, total_votes)
#print(avg_rank)

high_rank_id = ID[avg_rank > 6]
np.random.shuffle(high_rank_id)
#print(high_rank_id)
low_rank_id = ID[avg_rank < 4]
np.random.shuffle(low_rank_id)
#print(low_rank_id)

with open("train.txt", "w") as train_img_list:
	path = '/media/robin/DATA/caffe/data/ava/train/'
	for id in high_rank_id[:15000]:
		if os.path.isfile('images/' + str(id) + '.jpg'):
			im = Image.open('images/' + str(id) + '.jpg')
			if (im.size[0] < 256 or im.size[1] < 256):
				continue
			resize(im, id, path)
			train_img_list.write("%s %d\n" % (path+str(id)+'.jpg', 1))

	for id in low_rank_id[:15000]:
		if os.path.isfile('images/' + str(id) + '.jpg'):
			im = Image.open('images/' + str(id) + '.jpg')
			if (im.size[0] < 256 or im.size[1] < 256):
				continue
			resize(im, id, path)
			train_img_list.write("%s %d\n" % (path+str(id)+'.jpg', 0))

with open("test.txt", "w") as test_img_list:
	path = '/media/robin/DATA/caffe/data/ava/test/'
	for id in high_rank_id[15000:18000]:
		if os.path.isfile('images/' + str(id) + '.jpg'):
			im = Image.open('images/' + str(id) + '.jpg')
			if (im.size[0] < 256 or im.size[1] < 256):
				continue
			resize(im, id, path)
			test_img_list.write("%s %d\n" % (path+str(id)+'.jpg', 1))

	for id in low_rank_id[15000:18000]:
		if os.path.isfile('images/' + str(id) + '.jpg'):
			im = Image.open('images/' + str(id) + '.jpg')
			if (im.size[0] < 256 or im.size[1] < 256):
				continue
			resize(im, id, path)
			test_img_list.write("%s %d\n" % (path+str(id)+'.jpg', 0))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
