---
title: PIL example train (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'train'

Functions in program: 
* `def model_fn(features, labels, mode):`
* `def input_fn(mode):`

Modules used in program: 
* `import tensorflow as tf`
* `import os`
* `import cv2`
* `import uuid`
* `import matplotlib.pyplot as plt`

## python train

Python PIL example: train

```python
from data import get_webcam_input_fn, get_dataset
import matplotlib.pyplot as plt
import uuid
import cv2
import os
import tensorflow as tf


# Directories - PLEASE CONFIGURE IT
models_root = '/.../models'  # Output dir for the model and tensorboard logs
train_images_root = '/.../train-backgrounds'  # Train images (download random images from the internet)
valid_images_root = '/.../valid-backgrounds'  # Valid images (download random images from the internet)
logo_jpg_filename = '/.../logo.jpg'  # A jpg of the "Data Driven Montreal" logo (download https://cdn.evbuc.com/images/39269779/90171842189/2/logo.png , print, take a picture using your webcam, crop a square around the logo and save)


# Config
image_size = (240, 320, 3)
batch_size = 16
train_steps = 200


# Model ID
model_uuid = uuid.uuid4()
model_dir = os.path.join(models_root, f'{model_uuid}')


# Input Function
def input_fn(mode):

    if mode == tf.estimator.ModeKeys.TRAIN:
        dataset = get_dataset(image_size, train_images_root, logo_jpg_filename)
    else:
        dataset = get_dataset(image_size, valid_images_root, logo_jpg_filename)

    dataset = dataset \
        .batch(batch_size) \
        .prefetch(1)

    return dataset


# Model Function
def model_fn(features, labels, mode):

    with tf.name_scope('Input'):
        input_tensor = tf.identity(features)

    with tf.name_scope('Model'):
        net = input_tensor
        net = tf.layers.conv2d(net, 32, (3, 3), activation=tf.nn.relu)
        net = tf.layers.max_pooling2d(net, (2, 2), (2, 2))
        net = tf.layers.conv2d(net, 64, (3, 3), activation=tf.nn.relu)
        net = tf.layers.max_pooling2d(net, (2, 2), (2, 2))
        net = tf.layers.conv2d(net, 128, (3, 3), activation=tf.nn.relu)
        net = tf.layers.max_pooling2d(net, (2, 2), (2, 2))
        net = tf.layers.conv2d(net, 128, (3, 3), activation=tf.nn.relu)
        net = tf.layers.max_pooling2d(net, (2, 2), (2, 2))
        net = tf.layers.conv2d(net, 128, (3, 3), activation=tf.nn.relu)
        net = tf.layers.max_pooling2d(net, (2, 2), (2, 2))
        net = tf.layers.conv2d(net, 128, (3, 3), activation=tf.nn.relu)
        net = tf.layers.max_pooling2d(net, (2, 2), (2, 2))
        net = tf.layers.flatten(net)
        net = tf.layers.dense(net, 256, tf.nn.relu)
        net = tf.layers.dense(net, 512, tf.nn.relu)
        net = tf.layers.dense(net, 2, tf.nn.sigmoid, name='Predictions')
        predictions = {'predictions': net}

    if mode == tf.estimator.ModeKeys.PREDICT:
        predictions['image'] = input_tensor
        return tf.estimator.EstimatorSpec(mode=mode, predictions=predictions)

    with tf.name_scope('GroundTruth'):
        labels = tf.identity(labels, name='labels')

    with tf.name_scope('Loss'):
        loss = tf.losses.mean_squared_error(labels=labels, predictions=predictions['predictions'])

    with tf.name_scope('Summaries'):
        tf.summary.image('images', input_tensor)
        tf.summary.scalar('loss', loss)
        tf.summary.histogram('predictions', predictions['predictions'])
        tf.summary.histogram('targets', labels)

    if mode == tf.estimator.ModeKeys.EVAL:
        return tf.estimator.EstimatorSpec(mode=mode, loss=loss)

    with tf.name_scope('Optimizer'):
        optimizer = tf.train.AdamOptimizer()

        updates = tf.get_collection(tf.GraphKeys.UPDATE_OPS)
        with tf.control_dependencies(updates):
            train_op = optimizer.minimize(loss, global_step=tf.train.get_global_step())

        return tf.estimator.EstimatorSpec(mode=mode, loss=loss, train_op=train_op)


# Estimator
run_config = tf.estimator.RunConfig(
    model_dir=model_dir,
    save_summary_steps=10,
    save_checkpoints_secs=900,
    keep_checkpoint_max=1,
    log_step_count_steps=10
)
estimator = tf.estimator.Estimator(model_fn=model_fn, config=run_config)


# Training Loop
train_spec = tf.estimator.TrainSpec(input_fn=input_fn, max_steps=train_steps)
eval_spec = tf.estimator.EvalSpec(input_fn=input_fn, steps=20, name='eval', throttle_secs=30, start_delay_secs=1)

tf.estimator.train_and_evaluate(estimator, train_spec, eval_spec)


# Visualize predictions
predictions = estimator.predict(input_fn)
for i, p in zip(range(batch_size), predictions):
    plt.subplot(4, 4, i+1)
    img = p['image']
    coords = p['predictions']

    plt.imshow(img / 2.0 + 0.5)
    plt.plot(coords[0] * image_size[1], coords[1] * image_size[0], 'or')


# Webcam
cap, webcam_input_fn = get_webcam_input_fn(image_size)

# Predictions
predictions = estimator.predict(webcam_input_fn)


# Visualize Webcam
cv2.namedWindow('Press "q" to quit', cv2.WINDOW_NORMAL)
for p in predictions:

    # Webcam image + Predictions
    img = (p['image'][:, :, ::-1] + 1.0) / 2.0
    coords = p['predictions']

    # Draw prediction
    img = cv2.circle(img, (int(coords[0] * img.shape[1]), int(coords[1] * img.shape[0])), 2, (0, 255, 0), 19)

    # Display the resulting frame
    cv2.imshow('Press "q" to quit', img[:, ::-1, :])

    # Wait for 'q' click
    if cv2.waitKey(1) & 0xFF == ord('q'):
        cap.release()
        cv2.destroyAllWindows()
        break




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
