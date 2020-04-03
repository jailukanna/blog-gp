---
title: matplotlib example q5 video render (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'q5 video render'

Functions in program: 
* `def animate(frame):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib.pyplot as plt`
* `import matplotlib.image as mpimg`
* `import matplotlib.pyplot as plt`
* `import keras`
* `import numpy as np`
* `import catch`
* `import imp`
* `import qlearn`
* `import run_catch `
* `import os`
* `import sys`

## python q5 video render

Python matplotlib example: q5 video render

```python

# coding: utf-8

# # Video Rendering for Q5

# In[1]:

get_ipython().magic('matplotlib inline')

# traverses filesystem upwards until the "modules" folder is found
# stops when it reaches root
import sys
import os
target = 'modules'
curr_root = '.'
while os.path.realpath(curr_root) != os.path.realpath('/'):
    if os.access(target, os.F_OK):
        sys.path.append(target)
        break
    curr_root = '../' + curr_root
    target = '../' + target


# In[2]:

import run_catch 
import qlearn

import imp
import catch
import numpy as np
import keras
from keras.models import Sequential
from keras.layers.core import Dense
from keras.optimizers import adagrad
from keras.models import model_from_json
from keras.models import load_model


# In[3]:

imp.reload(run_catch)
train_catch = run_catch.train_catch
test_catch = run_catch.test_catch
imp.reload(catch)
Catch = catch.Catch
imp.reload(qlearn)
ExperienceReplay = qlearn.ExperienceReplay


# In[4]:

width = 13
height = 18
num_actions = 3
q_size = 5
mask =  False
ball_spawn_rate = 18
hidden_size = width * height * q_size

# load json and create model
with open('Q5_13x18_log_mask_False.json') as jsonfile:
    loaded_file = json.load(jsonfile)
    model = model_from_json(loaded_file)
    
model.load_weights('Q5_13x18_log_mask_False.h5')

c = Catch(
    screen_height=height, screen_width=width, output_buffer_size=q_size,
    game_over_conditions = {'ball_deletions': 1}, mask=mask, ball_spawn_rate=ball_spawn_rate)


c.reset()
tests, frames = test_catch(catch = c, model = model, test_on_games = 10, save_frames=True)


# In[54]:

import matplotlib.pyplot as plt
import matplotlib.image as mpimg
image = np.reshape(np.squeeze(frames[40]), newshape=(5, 18, 13))[0]
fig = plt.figure()
fig.subplots_adjust(hspace=0.0, wspace=0.0)

a = fig.add_subplot(1, 2, 1)
a.set_axis_off()


plt.imshow(image, cmap='gray')
image = np.reshape(np.squeeze(frames[50]), newshape=(5, 18, 13))[0]
a = fig.add_subplot(1, 2, 2)
a.set_axis_off()

plt.imshow(image, cmap='gray')
x = plt.show()


# In[104]:

# make the plot for animating

from matplotlib import animation, rc
import matplotlib.pyplot as plt
from IPython.display import HTML
from matplotlib.patches import Rectangle

# create figure for subplots
fig = plt.figure()
# remove as much spacing as possible; maybe
fig.subplots_adjust(hspace=0.1, wspace=0.1)

image_queue = np.reshape(frames[50], newshape=(5, 18, 13))

for i in range(len(image_queue)):
    a = fig.add_subplot(1, len(image_queue), i+1)
    a.set_axis_off()
    print(type(a))
    image = image_queue[i]
    plt.imshow(image, cmap='gray')
    
x = plt.show()


# In[130]:

# Create the animation
from matplotlib import animation, rc
from IPython.display import HTML
import matplotlib.pyplot as plt

# create figure for subplots
fig = plt.figure()
# remove as much spacing as possible; maybe
fig.subplots_adjust(hspace=0.1, wspace=0.1)
allaxis = fig.get_axes()

# animation function. This is called sequentially
def animate(frame):

    image = np.reshape(frame, newshape=(5, 18, 13))
    image = np.concatenate(image, axis=1)
    img = plt.imshow(image, cmap='gray', animated=True)

    return img,

# call the animator. blit=True means only re-draw the parts that have changed.
anim = animation.FuncAnimation(fig, animate,
                               frames=frames[1:101], interval=100, blit=True)
# Jupyter specific commands
HTML(anim.to_html5_video())


# In[ ]:





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
