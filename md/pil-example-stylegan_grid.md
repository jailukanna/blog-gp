---
title: pil example stylegan grid (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'stylegan grid'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import config`
* `import dnnlib.tflib as tflib`
* `import dnnlib`
* `import numpy as np`
* `import pickle`
* `import os`

## python stylegan grid

Python pil example: stylegan grid

```python
import os
import pickle
from PIL import Image
import numpy as np
import dnnlib
import dnnlib.tflib as tflib
import config
from encoder.generator_model import Generator

import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt
URL_FFHQ = 'https://drive.google.com/uc?id=1MEGjdvVpUsu1jB4zrXZN7Y4kBBOzizDQ'

tflib.init_tf()
with dnnlib.util.open_url(URL_FFHQ, cache_dir="cache") as f:
    generator_network, discriminator_network, Gs_network = pickle.load(f)

generator = Generator(Gs_network, batch_size=1, randomize_noise=False)


# Take these from https://github.com/Puzer/stylegan-encoder/tree/master/ffhq_dataset/latent_directions
smile_direction = np.load('ffhq_dataset/latent_directions/smile.npy')
gender_direction = np.load('ffhq_dataset/latent_directions/gender.npy')
age_direction = np.load('ffhq_dataset/latent_directions/age.npy')

np.random.seed(1)
random_person = np.random.normal(size=(18, 512)) / 6

n = 7
xaxis = np.linspace(-1, 1, n)[:, None, None, None]
yaxis = np.linspace(-1, 1, n)[None, :, None, None]

latents = xaxis * smile_direction + yaxis * gender_direction
latents = latents.reshape((-1, 18, 512))
latents += random_person[None, :, :]

imgs = []
for latent_vector in latents:
    latent_vector = latent_vector.reshape((1, 18, 512))
    generator.set_dlatents(latent_vector)
    img_array = generator.generate_images()[0]
    print(img_array.shape)
    imgs.append(img_array)

w = 1024
imgs = np.array(imgs)
imgs = imgs.reshape((n, n, w, w, 3))
big = np.zeros((n*w, n*w, 3), dtype=np.uint8)
for i in range(n):
    for j in range(n):
        big[i*w:(i+1)*w, j*w:(j+1)*w] = imgs[i, j]

pil_img = Image.fromarray(big,mode='RGB')
pil_img.save("vis.png")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
