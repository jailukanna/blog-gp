---
title: pil example ferg to hdf shuffled (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ferg to hdf shuffled'


Modules used in program: 
* `import random`
* `import numpy`
* `import PIL.Image`
* `import os`
* `import imageio`
* `import h5py`

## python ferg to hdf shuffled

Python pil example: ferg to hdf shuffled

```python
# (c) Tariq Rashid 2019
# GPL v2 

import h5py
import imageio
import os

import PIL.Image
import numpy

import random

##

emotions = ['surprise', 'sadness', 'neutral', 'joy', 'fear', 'disgust', 'anger']
names = ['ray', 'mery', 'malcolm', 'jules', 'bonnie', 'aia']

##

# accumulate all files first

files_list = []

for emotion in emotions:
    for name in names:
        dirname = 'fergdb/' + name+ '/' + name + '_' + emotion
        print(dirname)
        fnames = os.listdir(dirname)
        pathnames = [dirname+'/'+fname for fname in fnames]
        files_list.extend(pathnames)
        print(len(files_list))
        pass
    pass

##

# shuffle list then build hdf5

random.shuffle(files_list)

with h5py.File('fergdb/fergdb_64_shuffled.h5py', 'w') as hf:
    count = 1
    
    for entry in files_list:
        if entry.endswith("png"):
            #print(entry)
            img = imageio.imread(entry)

             # resize
            img = PIL.Image.fromarray(img).resize((64,64), resample=PIL.Image.BILINEAR)

            # remove alpha and use white background
            img2 = PIL.Image.new("RGB", img.size, "WHITE")
            img2.paste(img, (0, 0), img) 

            img2 = numpy.array(img2)
            hf.create_dataset(entry.split('/')[3], data=img2, compression="gzip", compression_opts=9)

            if (count%1000 == 0):
                print(count, img2.shape)
            count += 1
            pass
        pass
    pass



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
