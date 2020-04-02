---
title: PIL example Stylegan2-series (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Stylegan2-series'

Functions in program: 
* `def generate_series(network_pkl, seed, amount, truncation_psi, img_size=1024):`
* `def map(n, start1, stop1, start2, stop2): `
* `def generate_images(network_pkl, seeds, truncation_psi):`

Modules used in program: 
* `import pretrained_networks`
* `import math  `
* `import random`
* `import sys`
* `import re`
* `import dnnlib.tflib as tflib`
* `import dnnlib`
* `import PIL.Image`
* `import numpy as np`
* `import argparse`

## python Stylegan2-series

Python PIL example: Stylegan2-series

```python
import argparse
import numpy as np
import PIL.Image
import dnnlib
import dnnlib.tflib as tflib
import re
import sys
import random
import math  
from opensimplex import OpenSimplex
​
import pretrained_networks
​
#----------------------------------------------------------------------------
​
def generate_images(network_pkl, seeds, truncation_psi):
    print('Loading networks from "%s"...' % network_pkl)
    _G, _D, Gs = pretrained_networks.load_networks(network_pkl)
    noise_vars = [var for name, var in Gs.components.synthesis.vars.items() if name.startswith('noise')]
​
    Gs_kwargs = dnnlib.EasyDict()
    Gs_kwargs.output_transform = dict(func=tflib.convert_images_to_uint8, nchw_to_nhwc=True)
    Gs_kwargs.randomize_noise = False
    if truncation_psi is not None:
        Gs_kwargs.truncation_psi = truncation_psi
​
    for seed_idx, seed in enumerate(seeds):
        print('Generating image for seed %d (%d/%d) ...' % (seed, seed_idx, len(seeds)))
        rnd = np.random.RandomState(seed)
        z = rnd.randn(1, *Gs.input_shape[1:]) # [minibatch, component]
        tflib.set_vars({var: rnd.randn(*var.shape.as_list()) for var in noise_vars}) # [height, width]
        images = Gs.run(z, None, **Gs_kwargs) # [minibatch, height, width, channel]
        PIL.Image.fromarray(images[0], 'RGB').save(dnnlib.make_run_dir_path('seed%04d.png' % seed))
​
#----------------------------------------------------------------------------
​
def map(n, start1, stop1, start2, stop2): 
  return ((n-start1)/(stop1-start1))*(stop2-start2)+start2
​
​
class NoiseLoop():  
    def __init__(self, diameter, min, max):
        self.diameter = diameter
        self.min = min
        self.max = max
        self.cx = random.randint(0,1000)
        self.cy = random.randint(0,1000)
        self.simplex = OpenSimplex()
​
    def value(self, a):
        # map(value, start1, stop1, start2, stop2, [withinBounds])
        xoff = map(math.cos(a), -1, 1, self.cx, self.cx + self.diameter)
        yoff = map(math.sin(a), -1, 1, self.cy, self.cy + self.diameter)
        r = self.simplex.noise2d(xoff, yoff)
        return map(r, -1, 1, self.min, self.max)
​
​
def generate_series(network_pkl, seed, amount, truncation_psi, img_size=1024):
​
    print('Loading networks from "%s"...' % network_pkl)
    _G, _D, Gs = pretrained_networks.load_networks(network_pkl)
    noise_vars = [var for name, var in Gs.components.synthesis.vars.items() if name.startswith('noise')]
​
    Gs_kwargs = dnnlib.EasyDict()
    Gs_kwargs.output_transform = dict(func=tflib.convert_images_to_uint8, nchw_to_nhwc=True)
    Gs_kwargs.randomize_noise = False
    if truncation_psi is not None:
        Gs_kwargs.truncation_psi = truncation_psi
       
​
    # Generate random vector from the seed
    rnd_seed = np.random.RandomState(seed)
    z = rnd_seed.randn(1, *Gs.input_shape[1:]) # [minibatch, component]
​
    # Generate the first image
    tflib.set_vars({var: rnd_seed.randn(*var.shape.as_list()) for var in noise_vars}) # [height, width]
    images = Gs.run(z, None, **Gs_kwargs) # [minibatch, height, width, channel]
    first_array = images[0]
​
    # Save the first image
    PIL.Image.fromarray(first_array, 'RGB').save(dnnlib.make_run_dir_path('seed%04d-0.png' % seed))   
​
    # Setup next images
    angle = 0 
    n = []
​
    # setup noise
    for i in range(0, img_size):
        n.append(NoiseLoop(20, -1, 1))
​
    # Generate next images
    images_list = list(range(0, amount))
    for image_idx in images_list:
​
        print('z.shape {}'.format(z.shape))
​
        for i in range(0, int(img_size / 2)):
            z[0][i] = n[i].value(angle); 
        
        da = (math.pi * 2) / (24 * 60); #- not sure why these values are used (1440 = 360*4)
        angle += da
        
        print('Generating images for seed %d (%d/%d) ...' % (seed, image_idx, len(images_list)))
​
        images = Gs.run(z, None, **Gs_kwargs) # [minibatch, height, width, channel]
        image_arr = images[0]
​
        # Save the first image
        PIL.Image.fromarray(image_arr, 'RGB').save(dnnlib.make_run_dir_path('seed-{}-{}.png'.format(seed, image_idx)))
    
    
    ...
       

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
