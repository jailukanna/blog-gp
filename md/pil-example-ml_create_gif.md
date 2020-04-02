---
title: pil example ml create gif (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ml create gif'

Functions in program: 
* `def save_image(noise_data, img_uuid):`
* `def step(noise_init, noise_end, start_uuid, steps):`
* `def generate_noise(n_samples, max_variation = 1):`
* `def print_time():`

Modules used in program: 
* `import time`
* `import constants as P`
* `import os, time`
* `import uuid`
* `import numpy as np`
* `import keras.backend as K`

## python ml create gif

Python pil example: ml create gif

```python
from keras.models import load_model
import keras.backend as K
from PIL import Image
import numpy as np
import uuid
import os, time
from random import randint
import constants as P

GENERATOR_MODEL = 'model.h5'

GIF_EXPORT_DIR = "GIF/"
GIF_SIZE = 400, 400
GIF_TOTAL_STEPS = 10
NORMAL_VARIATION = 0.01
STEPS_SIZE = 12, 40
#STEPS_SIZE = 4, 8
STEPS_NUMBER = 4 # 2 minimum in order to create a loop

# TIMING CONTROL
import time
start_time = time.time()
def print_time():
    elapsed_time = time.time() - start_time
    mins = int(elapsed_time / 60)
    secs = elapsed_time - (mins * 60)
    print("Accumulative time: %02d:%02d" % (mins, int(secs % 60)))

# Preload our model
print("Loading model..")
print_time()


generator = load_model(GENERATOR_MODEL, compile=False)
print("Loading model.. DONE!")
print_time()

def generate_noise(n_samples, max_variation = 1):
    X = np.random.normal(0, max_variation, size=(n_samples, P.NOISE_SHAPE))
    return X

def step(noise_init, noise_end, start_uuid, steps):
    img_uuid = start_uuid
    noise_data = np.array(noise_init)
    while (img_uuid < start_uuid + steps):
        for i in range(len(noise_init[0])):
            add_extra = (noise_end[0][i] - noise_init[0][i]) / steps
            noise_data[0][i] += add_extra
            #print(i, noise_data[0][i], add_extra)
        img_uuid += 1
        save_image(noise_data, img_uuid)
    return noise_data, img_uuid

def save_image(noise_data, img_uuid):
    img_batch = generator.predict(noise_data)
    print("Painting created..")
    print_time()

    pil_image = Image.fromarray(np.asarray(img_batch[0]*255, np.uint8))
    pil_image = pil_image.resize(GIF_SIZE, Image.ANTIALIAS)
    print("Painting", img_uuid, "to image DONE!")
    print_time()

    # ORIGINAL SAVE
    pil_image.save(GIF_EXPORT_DIR + str(img_uuid) + ".png")
    return noise_data, img_uuid

img_uuid = 0
noise_start = generate_noise(1)
noise_next = noise_start

## ONE IMAGE TO ANOTHER
for i in range(STEPS_NUMBER - 1):
    step_size = randint(STEPS_SIZE[0], STEPS_SIZE[1])
    noise_next, img_uuid = step(noise_next, generate_noise(1), img_uuid, step_size)
    print("-- NEW STEP --")

## PROGRSSIVE STEPS, NOT WORKING, IT BURNS THE IMAGE :(
#for i in range(STEPS_NUMBER - 1):
#    step_size = randint(STEPS_SIZE[0], STEPS_SIZE[1])
#    start_uuid = img_uuid
#    noise_temp = noise_next + generate_noise(1, NORMAL_VARIATION)
#    while (img_uuid < start_uuid + step_size):
#        noise_next, img_uuid = save_image(noise_next + noise_temp, img_uuid+1)
#    
#    print("-- NEW STEP --")

## ORGANIC MOVEMENT
#for i in range(GIF_TOTAL_STEPS):
#    noise_temp = noise_next + generate_noise(1, NORMAL_VARIATION)
#    noise_next, img_uuid = save_image(noise_temp, img_uuid+1)

print("-- FINAL STEP --")
step_size = randint(STEPS_SIZE[0], STEPS_SIZE[1])
step(noise_next, noise_start, img_uuid, step_size)

K.clear_session()
print_time()
print("ALL DONE, THANKS!")
print("YOU CAN CREATE YOR GIF NOW WITH: convert -delay 13 -loop 0 'GIF/%d.png[0-99999]' exported.gif ")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
