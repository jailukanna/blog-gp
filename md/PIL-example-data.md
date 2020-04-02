---
title: PIL example data (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'data'

Functions in program: 
* `def _data_generator(random_seed=None,`
* `def get_webcam_input_fn(image_size):`
* `def get_dataset(image_size, images_root, logo_jpg_filename):`

Modules used in program: 
* `import tensorflow as tf`
* `import cv2`
* `import numpy as np`
* `import random`
* `import PIL.ImageEnhance`
* `import PIL.ImageOps`
* `import PIL.ImageDraw`
* `import PIL.Image`
* `import os`
* `import functools`

## python data

Python PIL example: data

```python
import functools
import os
import PIL.Image
import PIL.ImageDraw
import PIL.ImageOps
import PIL.ImageEnhance
import random
import numpy as np
import cv2
import tensorflow as tf


def get_dataset(image_size, images_root, logo_jpg_filename):
    generator = functools.partial(_data_generator, textures_dir=images_root, logo_filename=logo_jpg_filename)
    dataset = tf.data.Dataset.from_generator(generator,
                                             (tf.float32,
                                              tf.float32),
                                             (tf.TensorShape(image_size),
                                              tf.TensorShape([2])))
    return dataset


def get_webcam_input_fn(image_size):
    cap = cv2.VideoCapture(0)

    def webcam_input_fn(mode):

        def get_webcam_feed():
            while True:
                ret, frame = cap.read()

                if not ret:
                    break

                # Resize frame
                frame_resized = cv2.resize(frame, dsize=(image_size[1], image_size[0]))

                # Preprocess frame
                frame_preprocessed = (2.0 * frame_resized[:, :, ::-1] / 255.0 - 1.0).astype(np.float32)

                yield frame_preprocessed

        webcam_dataset = tf.data.Dataset.from_generator(get_webcam_feed,
                                                        tf.float32,
                                                        tf.TensorShape(image_size))
        webcam_dataset = webcam_dataset.batch(1)

        return webcam_dataset

    return cap, webcam_input_fn


def _data_generator(random_seed=None,
                    border=0,
                    logo_size_min=70,
                    image_size=(320, 240),
                    textures_dir=None,
                    logo_filename=None):
    # Thread-safe random
    local_random = random.Random()
    local_random.seed(random_seed)

    # Definitions
    textures_filenames = [os.path.join(textures_dir, f) for f in os.listdir(textures_dir)]
    # kernel = np.ones((2, 2), np.uint8)

    # Load and process logo
    logo_image_orig = PIL.ImageOps.expand(PIL.Image.open(logo_filename), border=border, fill='white').convert('RGBA')
    logo_image = PIL.Image.new('RGBA', logo_image_orig.size, color=(255, 255, 255))
    logo_image.alpha_composite(logo_image_orig)

    logo_circle = PIL.Image.new('L', logo_image.size, color=0)
    draw = PIL.ImageDraw.Draw(logo_circle)
    for i in range(0, 40):
        draw.ellipse((i, i, logo_image.size[0]-i, logo_image.size[1]-i), fill=int(255 / 40 * i), outline=0)
    logo_image.putalpha(logo_circle)

    while True:
        # Create background
        texture_filename = local_random.choice(textures_filenames)
        texture_image = PIL.Image.open(texture_filename).convert("RGBA")
        w, h = (0.5 + local_random.random() * 0.5, 0.5 + local_random.random() * 0.5)
        x1 = local_random.random() * (1 - w)
        x2 = x1 + w
        y1 = local_random.random() * (1 - h)
        y2 = y1 + h

        background_image = texture_image \
            .crop((x1 * texture_image.size[0], y1 * texture_image.size[1],
                   x2 * texture_image.size[0], y2 * texture_image.size[1])) \
            .resize(image_size, resample=PIL.Image.BILINEAR)
        brightness_factor = local_random.random() * 1.6 + 0.2
        brightness = PIL.ImageEnhance.Brightness(background_image)
        background_image = brightness.enhance(brightness_factor)

        # Create logo to paste
        brightness_factor = local_random.random() * 1.4 + 0.2
        color_factor = local_random.random()
        angle = local_random.randrange(0, 360)
        logo_size = local_random.randrange(logo_size_min, min(image_size) * 0.5)
        logo_origin = [local_random.randrange(0, image_size[0] - logo_size),
                       local_random.randrange(0, image_size[1] - logo_size)]

        logo_to_paste = logo_image \
            .rotate(angle) \
            .resize((logo_size, logo_size), resample=PIL.Image.BILINEAR)
        brightness = PIL.ImageEnhance.Brightness(logo_to_paste)
        logo_to_paste = brightness.enhance(brightness_factor)
        color = PIL.ImageEnhance.Color(logo_to_paste)
        logo_to_paste = color.enhance(color_factor)

        squeeze_factors = (local_random.random() * 0.3 + 0.7,
                           local_random.random() * 0.3 + 0.7)
        logo_shift = (int(logo_to_paste.size[0] * (1.0 - squeeze_factors[0]) / 2),
                      int(logo_to_paste.size[1] * (1.0 - squeeze_factors[1]) / 2))
        logo_to_paste = logo_to_paste.resize((int(logo_to_paste.size[0] * squeeze_factors[0]),
                                              int(logo_to_paste.size[1] * squeeze_factors[1])))
        logo_origin[0] += logo_shift[0]
        logo_origin[1] += logo_shift[1]

        background_image.paste(logo_to_paste, logo_origin, logo_to_paste)
        background_image = np.array(background_image.convert('RGB'))

        targets = np.array([(logo_origin[0] + logo_origin[2]) / 2.0 / image_size[0],
                            (logo_origin[1] + logo_origin[3]) / 2.0 / image_size[1],])
        inputs = 2.0 * background_image / 255.0 - 1.0

        yield inputs, targets





```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
