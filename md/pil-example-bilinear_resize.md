---
title: pil example bilinear resize (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'bilinear resize'

Functions in program: 
* `def test_bilinear_resize():`
* `def bilinear_resize(inputs, factor):`
* `def interpolate(inputs, x, y, out_size, exclude_outside=False):`
* `def repeat(x, n_repeats):`
* `def gen_interp_grid(width, height, factor):`
* `def tf_linspace2(start, stop, num, endpoint=True):`

Modules used in program: 
* `import tensorflow as tf`

## python bilinear resize

Python pil example: bilinear resize

```python
import tensorflow as tf


def tf_linspace2(start, stop, num, endpoint=True):
  start = tf.to_float(start)
  stop = tf.to_float(stop)
  if endpoint:
    return tf.linspace(start, stop, num)
  else:
    return tf.linspace(start, stop, num+1)[:-1]


def gen_interp_grid(width, height, factor):
  if factor.dtype.is_integer:
    new_height = height * factor
    new_width = width * factor
  else:
    new_height = tf.to_int32(tf.to_float(height) * factor)
    new_width = tf.to_int32(tf.to_float(width) * factor)

  half_step = 1 / (2 * factor)
  xx = tf_linspace2(0,  width,  new_width, endpoint=False) - 0.5 + half_step
  yy = tf_linspace2(0, height, new_height, endpoint=False) - 0.5 + half_step


  x = tf.matmul(tf.ones(shape=tf.stack([new_height, 1])),
                tf.expand_dims(xx, 0))
  y = tf.matmul(tf.expand_dims(yy, 1),
                tf.ones(shape=tf.stack([1, new_width])))

  return x, y


def repeat(x, n_repeats):
  with tf.variable_scope('repeat'):
    rep = tf.transpose(
        tf.expand_dims(tf.ones(shape=tf.stack([n_repeats, ])), 1), [1, 0])
    rep = tf.cast(rep, 'int32') # Q: should we use `x.dtype` instead of 'int32' ?
    x = tf.matmul(tf.reshape(x, (-1, 1)), rep)
    return tf.reshape(x, [-1])


def interpolate(inputs, x, y, out_size, exclude_outside=False):
  with tf.variable_scope('interpolate'):
    # constants
    num_batch = tf.shape(inputs)[0]
    height    = tf.shape(inputs)[1]
    width     = tf.shape(inputs)[2]
    channels  = tf.shape(inputs)[3]

    x = tf.cast(x, 'float32')
    y = tf.cast(y, 'float32')
    out_height = out_size[1]
    out_width  = out_size[2]
    zero = tf.zeros([], dtype='int32')
    max_y = tf.cast(tf.shape(inputs)[1] - 1, 'int32')
    max_x = tf.cast(tf.shape(inputs)[2] - 1, 'int32')

    # do sampling
    x0 = tf.cast(tf.floor(x), 'int32')
    x1 = x0 + 1
    y0 = tf.cast(tf.floor(y), 'int32')
    y1 = y0 + 1

    x0_clipped = tf.clip_by_value(x0, zero, max_x)
    x1_clipped = tf.clip_by_value(x1, zero, max_x)
    y0_clipped = tf.clip_by_value(y0, zero, max_y)
    y1_clipped = tf.clip_by_value(y1, zero, max_y)

    dim2 = width
    dim1 = width * height
    base = repeat(tf.range(num_batch) * dim1, out_height * out_width)
    base_y0 = base + y0_clipped * dim2
    base_y1 = base + y1_clipped * dim2
    idx_a = base_y0 + x0_clipped
    idx_b = base_y1 + x0_clipped
    idx_c = base_y0 + x1_clipped
    idx_d = base_y1 + x1_clipped

    # use indices to lookup pixels in the flat image and restore channels dim
    im_flat = tf.reshape(inputs, tf.stack([-1, channels]))
    im_flat = tf.cast(im_flat, 'float32')
    Ia = tf.gather(im_flat, idx_a)
    Ib = tf.gather(im_flat, idx_b)
    Ic = tf.gather(im_flat, idx_c)
    Id = tf.gather(im_flat, idx_d)

    if exclude_outside:
      # and finally calculate interpolated values
      x0_f = tf.cast(x0_clipped, 'float32')
      x1_f = tf.cast(x1_clipped, 'float32')
      y0_f = tf.cast(y0_clipped, 'float32')
      y1_f = tf.cast(y1_clipped, 'float32')

      wa = tf.expand_dims(((x1_f - x) * (y1_f - y)), 1)
      wb = tf.expand_dims(((x1_f - x) * (y - y0_f)), 1)
      wc = tf.expand_dims(((x - x0_f) * (y1_f - y)), 1)
      wd = tf.expand_dims(((x - x0_f) * (y - y0_f)), 1)
      output = tf.add_n([wa * Ia, wb * Ib, wc * Ic, wd * Id])
    else:
      # and finally calculate interpolated values
      x0_f = tf.cast(x0, 'float32')
      x1_f = tf.cast(x1, 'float32')
      y0_f = tf.cast(y0, 'float32')
      y1_f = tf.cast(y1, 'float32')

      dx_r = tf.nn.relu(x1_f - x)
      dx_l = tf.nn.relu(x - x0_f)
      dy_b = tf.nn.relu(y1_f - y)
      dy_t = tf.nn.relu(y - y0_f)
      wa = tf.expand_dims((dx_r * dy_b), 1)
      wb = tf.expand_dims((dx_r * dy_t), 1)
      wc = tf.expand_dims((dx_l * dy_b), 1)
      wd = tf.expand_dims((dx_l * dy_t), 1)
      output = tf.add_n([wa * Ia, wb * Ib, wc * Ic, wd * Id]) \
               / tf.add_n([wa, wb, wc, wd]) # TODO: check division by zero cannot happen

    return output


def bilinear_resize(inputs, factor):
  """ Sub-differentiable bilinear resize. """
  inp_size = tf.shape(inputs)
  num_batch    = inp_size[0]
  height       = inp_size[1]
  width        = inp_size[2]
  num_channels = inp_size[3]

  factor = tf.constant(factor, 'float32')
  if factor.dtype.is_integer:
    new_height = height * factor
    new_width = width * factor
  else:
    new_height = tf.to_int32(tf.to_float(height) * factor)
    new_width = tf.to_int32(tf.to_float(width) * factor)
  out_size = tf.stack([num_batch, new_height, new_width, num_channels])

  inputs = tf.cast(inputs, tf.float32)

  # Compute bilinear upsampling grid for a given factor
  x, y = gen_interp_grid(width, height, factor)
  x_flat = tf.reshape(tf.tile(tf.reshape(x, [1, -1]), [num_batch, 1]), [-1])
  y_flat = tf.reshape(tf.tile(tf.reshape(y, [1, -1]), [num_batch, 1]), [-1])

  # Interpolate values at (x_s, y_s)
  output_flat = interpolate(inputs, x_flat, y_flat, out_size)
  output = tf.reshape(output_flat, out_size)

  return output


def test_bilinear_resize():
  import numpy as np
  from PIL import Image
  from skimage import transform
  import cv2
  import matplotlib.pyplot as plt

  image = tf.image.decode_png(
      tf.gfile.FastGFile('test_16x16.png', 'rb').read(),
      channels=3)

  with tf.Session():
    img = image.eval()
    for p in range(-2, 4):
      factor = 2**p
      print('factor =', factor)

      new_size = [dim*factor for dim in img.shape[:2]]
      if any([dim.is_integer() if not isinstance(dim, int) else False for dim in new_size]):
        print(('Warning: the new size are not integer and will then be cast.'))
        new_size = [int(dim) for dim in new_size]

      image_up = bilinear_resize(tf.expand_dims(image, 0), factor=factor)[0].eval().astype(np.uint8)
      image_up_pil = np.asarray(Image.fromarray(img).resize(new_size, resample=Image.BILINEAR))
      image_up_ski1 = (transform.resize(img, new_size, order=1, anti_aliasing=False)*255.).astype(np.uint8)
      image_up_ski2 = (transform.resize(img, new_size, order=1, anti_aliasing=True)*255.).astype(np.uint8)
      image_up_cv2 = cv2.resize(image_up, tuple(new_size))

      fig = plt.figure(1)
      fig.add_subplot(2,3,1)
      plt.imshow(img)
      plt.title('input 16x16 image')
      fig.add_subplot(2,3,2)
      plt.imshow(image_up_ski1)
      plt.title('skimage.transform.resize(*kwargs, anti_aliasing=False)')
      fig.add_subplot(2,3,3)
      plt.imshow(image_up_ski2)
      plt.title('skimage.transform.resize(*kwargs, anti_aliasing=True)')
      fig.add_subplot(2,3,4)
      plt.imshow(image_up)
      plt.title('sub-differentiable bilinear resize (ours)')
      fig.add_subplot(2,3,5)
      plt.imshow(image_up_cv2)
      plt.title('cv2.resize')
      fig.add_subplot(2,3,6)
      plt.imshow(image_up_pil)
      plt.title('PIL.Image.resize')

      plt.show()


if __name__ == "__main__":
  test_bilinear_resize()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
