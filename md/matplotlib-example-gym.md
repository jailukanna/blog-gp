---
title: matplotlib example gym (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'gym'


Modules used in program: 
* `import gym`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import gym`
* `import gym`
* `import matplotlib.pyplot as plt`
* `import serverutils`

## python gym

Python matplotlib example: gym

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import serverutils
serverutils.change_matplotlib_backend_to_agg()

import matplotlib.pyplot as plt
import gym
env = gym.make('SpaceInvaders-v0')
render = lambda : plt.imshow(env.render(mode='rgb_array'))
env.reset()
render()

#####
env = gym.make("Taxi-v2")
observation = env.reset()

for _ in range(1000):
  env.render()
  action = env.action_space.sample() # your agent here (this takes random actions)
  observation, reward, done, info = env.step(action)

env.__dict__["action_space"]
env.__dict__["observation_space"].sample()


from gym import wrappers
env = gym.make("Taxi-v2")
env = wrappers.Monitor(env, "./")
observation = env.reset()

for _ in range(1000):
  env.render()
  action = env.action_space.sample() # your agent here (this takes random actions)
  observation, reward, done, info = env.step(action)
  if done:
    print("end")
    break


######

import gym
from gym import wrappers
import matplotlib
matplotlib.use('agg')
#matplotlib.use('agg') 
import matplotlib.pyplot as plt
matplotlib.get_backend()

env = gym.make('CartPole-v0')
env = wrappers.Monitor(env, './CartPole2', force=True)
for i_episode in range(20):
    observation = env.reset()
    for t in range(100):
        env.render()
        print(observation)
        action = env.action_space.sample()
        observation, reward, done, info = env.step(action)
        if done:
            print("Episode finished after {} timesteps".format(t+1))
            break

###
trained_model = history.__dict__["model"]
env = wrappers.Monitor(env, './CartPole2', force=True)

for i_episode in range(20):
    observation = env.reset()
    for t in range(100):
        env.render()
        print(observation)
        action = model.forward(observation)
        observation, reward, done, info = env.step(action)
        if done:
            print("Episode finished after {} timesteps".format(t+1))
            break

###


import numpy as np
import gym
from gym import wrappers

from keras.models import Sequential
from keras.layers import Dense, Activation, Flatten
from keras.optimizers import Adam

from rl.agents.dqn import DQNAgent
from rl.policy import BoltzmannQPolicy
from rl.memory import SequentialMemory


ENV_NAME = 'CartPole-v0'


# Get the environment and extract the number of actions.
env = gym.make(ENV_NAME)
env = wrappers.Monitor(env, './CartPole')

np.random.seed(123)
env.seed(123)
nb_actions = env.action_space.n

# Next, we build a very simple model.
model = Sequential()
model.add(Flatten(input_shape=(1,) + env.observation_space.shape))
model.add(Dense(16))
model.add(Activation('relu'))
model.add(Dense(16))
model.add(Activation('relu'))
model.add(Dense(16))
model.add(Activation('relu'))
model.add(Dense(nb_actions))
model.add(Activation('linear'))
print(model.summary())

# Finally, we configure and compile our agent. You can use every built-in Keras optimizer and
# even the metrics!
memory = SequentialMemory(limit=50000, window_length=1)
policy = BoltzmannQPolicy()
dqn = DQNAgent(model=model, nb_actions=nb_actions, memory=memory, nb_steps_warmup=10,
               target_model_update=1e-2, policy=policy)
dqn.compile(Adam(lr=1e-3), metrics=['mae'])

# Okay, now it's time to learn something! We visualize the training here for show, but this
# slows down training quite a lot. You can always safely abort the training prematurely using
# Ctrl + C.
dqn.fit(env, nb_steps=50000, visualize=True, verbose=2)

# After training is done, we save the final weights.
dqn.save_weights('dqn_{}_weights.h5f'.format(ENV_NAME), overwrite=True)

# Finally, evaluate our algorithm for 5 episodes.
dqn.test(env, nb_episodes=5, visualize=True)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
