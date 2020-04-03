---
title: matplotlib example toy linear regression (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'toy linear regression'


Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`
* `import tensorflow as tf`

## python toy linear regression

Python matplotlib example: toy linear regression

```python
import tensorflow as tf
config = tf.ConfigProto()
config.gpu_options.allow_growth = True
import numpy as np
import matplotlib
from matplotlib import pyplot as plt
%matplotlib inline

class linear_reg(tf.keras.Model):
    def __init__(self):
        super(linear_reg, self).__init__(name='linear_reg')
        linear_reg_Model = tf.keras.Sequential([
            tf.keras.layers.Dense(1, input_shape=(1,), kernel_initializer='ones', bias_initializer='zeros'),
        ])
        self.linear_reg_Model = linear_reg_Model

    def __call__(self, x, training=False):
        y = self.linear_reg_Model(x)
        return y

sess = tf.Session(config=config)
tf.keras.backend.set_session(sess)

lr_model = linear_reg()

plt.figure(figsize=(16,5))

x_t = 100*np.random.randn(1000, 1)
y_t = 2 * x_t
x_v = 100*np.random.randn(1000, 1)
y_v = 2 * x_v

plt.scatter(x_t, y_t)
plt.scatter(x_v, y_v)
plt.show()


lr_model.compile(tf.keras.optimizers.SGD(lr=1e-5),
              loss=tf.keras.metrics.mean_squared_error)

history = lr_model.fit(x=x_t, y=y_t, 
                    steps_per_epoch=1, epochs=50,
                    validation_data = (x_v, y_v),
                    validation_steps=2,)

lr_model.get_weights() # [1.9999878], [8.888327e-05]
lr_model.summary()

import matplotlib.pyplot as plt
import numpy as np
%matplotlib inline

# Loss
plt.figure(figsize=(16,5))
loss = history.history['loss']
val_loss = history.history['val_loss']
epochs = range(1, len(loss) + 1)
plt.plot(epochs, loss, '--bo', label='Training loss')
plt.plot(epochs, val_loss, '-rs', label='Validation loss')
plt.title('Training and validation loss',  fontsize=18)
plt.xlabel('Epochs', fontsize=18)
plt.ylabel('Loss', fontsize=18)
plt.legend(fontsize=15)
plt.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
