---
title: PIL example extract (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'extract'

Functions in program: 
* `def extract_images(in_dir, out_dir):`

Modules used in program: 
* `import fire`
* `import os`
* `import numpy as np`
* `import PIL`
* `import logging`
* `import torch`
* `import torch.nn as nn`
* `import torchvision.transforms as transforms`
* `import torchvision.models as models`

## python extract

Python PIL example: extract

```python
"""
Extracts images from TurningWeaknessIntoStrength working directories created by attack.py
"""

import torchvision.models as models
import torchvision.transforms as transforms
import torch.nn as nn
import torch
import logging
import PIL
from PIL import Image
import numpy as np
logging.basicConfig(level=logging.INFO)

pilTrans = transforms.ToPILImage()

from glob import glob
from tqdm import tqdm
import os
import fire



def extract_images(in_dir, out_dir):
    """
    Extract torch tensor images from TurningWeaknessIntoStrength File organization.
    Mirrors tensors in in_dir to the same organization in out_dir
    """
    in_dir = os.path.abspath(in_dir)
    out_dir = os.path.abspath(out_dir)
    images = []
    for d in range(5):
        globber = in_dir + "/*"*d  + '/*.pt'
        images.extend(glob(globber))
    logging.info("Got %s pt from %s" % (len(images), in_dir))
    for i, image_path in tqdm(enumerate(images)):
        # There is a bunch of print(spam in here that is unnecessary but since this is a pure research script I have left them.)
        # PATHS SOURCE AND TARGETS
        base_dir = os.path.dirname(image_path)
        base_file = os.path.split(image_path)[-1]
        base_image_path = os.path.splitext(base_file)[0]
        if "label" in base_image_path:
            continue
        print(base_image_path)
        
        target_dir = base_dir.replace(in_dir, out_dir)
        target_file = os.path.join(target_dir, base_image_path + ".png")
        print(target_file)
        os.makedirs(os.path.dirname(target_file),exist_ok=1)
        #label_path = os.path.join(in_dir, image_path.replace("img.pt", "label.pt"))
        # DATA
        view_data = torch.load(image_path)
        #label_data = torch.load(label_path)
        #label = label_data.numpy()
        #x  = view_data.cpu().numpy()
        with torch.no_grad():  #necessary to extract cw images
            x = view_data.cpu().numpy()
        print(np.quantile(x,[0, 0.5, 1]))  # x is between 0 and 1
        import skimage
        from skimage.io import imsave
        x =  x[0].transpose((1,2,0))  # x is (1,3,height, width)
        print(x.shape)
        assert(x.shape[2] == 3)  # x is now 3 channel RGB as thee last dim
        x = skimage.img_as_ubyte(x[:])
        imsave(target_file, x)

if __name__ == "__main__":
    # use as 
    # python extract.py <in_dir> <out_dir>
     fire.Fire(extract_images)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
