---
title: matplotlib example cls tfrecords read write (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'cls tfrecords read write'

Functions in program: 
* `def _convert(im, filename=None):`
* `def _readfiles(files, tqdm=lambda x: x):`
* `def tf_parse_cls_example(serialized_example, convert_fn=None):`
* `def cls_examples_iterator(input_file,`
* `def write_image_to_cls_tfrecords(images, labels, output_file=None,`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import matplotlib.pyplot as plt`
* `import cv2`
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import tensorflow as tf`
* `import cv2`
* `import os`

## python cls tfrecords read write

Python matplotlib example: cls tfrecords read write

```python
# -*- coding: utf-8 -*-
# ---
# jupyter:
#   jupytext:
#     text_representation:
#       extension: .py
#       format_name: percent
#       format_version: '1.2'
#       jupytext_version: 1.1.6
#   kernelspec:
#     display_name: TFVDL-dev [conda env:TFVDL]
#     language: python
#     name: conda-env-TFVDL-py
# ---

# %%
##
import os
import cv2
import tensorflow as tf


def write_image_to_cls_tfrecords(images, labels, output_file=None,
                                 convert_fn=None,
                                 tqdm=lambda i, total=None: i,
                                 num_records=None):
    """
    텐서플로우 그래프를 사용하지 않고, 이미지와 레이블값들을
    tfrecord 파일에 저장하는 함수.
    저장전에 이미지 변환 (crop, resize 등등) 필요시 convert_fn 함수를
    지정하면 변환하면서 tfrecord 파일에 저장함.
    
    tfrecord 파일은 tf.train.Example protobuf 객체의 시퀀스로 되어 있고,
    classification task 에서는 tf.train.Example 객체의 속성 중 다음만
    사용함:
    
    image/encoded: jpg, png 등으로 인코딩 된 이미지 파일의 바이너리 데이터
    image/format: 이미지 파일의 확장자. `.' 는 포함하지 않음.
                  예: 'jpg', 'png'
    image/class/label: 해당 이미지의 레이블 정수값 (int64)
    image/height: 이미지 height 정수값 (int64)
    image/width: 이미지 width 정수값 (int64)
    """
    assert output_file, (output_file,)

    def int64_feature(values):
        return tf.train.Feature(int64_list=tf.train.Int64List(value=[values]))

    def bytes_feature(values):
        return tf.train.Feature(bytes_list=tf.train.BytesList(value=[values]))

    def image_to_tfexample(image_data, image_format, height, width, class_id):
        return tf.train.Example(features=tf.train.Features(feature={
            'image/encoded': bytes_feature(image_data),
            'image/format': bytes_feature(image_format),
            'image/class/label': int64_feature(class_id),
            'image/height': int64_feature(height),
            'image/width': int64_feature(width),
        }))

    with tf.python_io.TFRecordWriter(output_file) as tfr_writer:
        for i, (im, label) in enumerate(tqdm(zip(images, labels),
                                             total=num_records)):
            ext = '.png'

            if callable(convert_fn):
                im = convert_fn(im)
                
            # cv2.imencode() 를 호출하기전에 RGB->BGR 변환 필요
            if len(im.shape) > 2:
                im = im[:, :, ::-1] 
            _, im_encoded_arr = cv2.imencode(ext, im)
            im_encoded = im_encoded_arr.tobytes()

            ex_height, ex_width = im.shape[:2]
            ex_ext = bytes(ext[1:], 'ascii')
            ex_cls = label

            example = image_to_tfexample(
                im_encoded, ex_ext, ex_height, ex_width, ex_cls)

            tfr_writer.write(example.SerializeToString())


def cls_examples_iterator(input_file,
                          convert_fn=None,
                          tqdm=lambda i, total=None: i,
                          num_records=None):
    """
    텐서플로우 그래프를 거치지 않고 classification task 용으로
    만들어진 tfrecord 파일을 읽는 generator 함수.
    
    사용방법 예시:
        num_records, gen = cls_examples_iterator('sample.tfrecord')
        for im, label in gen():
            ... do something with im, label

    - num_records 인자를 지정하면 앞에서부터 해당 갯수만큼만 읽어옴.
    - 읽으면서 crop, resize 등 이미지 처리 필요한 경우 convert_fn 인자로
      지정할 수 있음.
    
    """
    if num_records is None:
        count = 0
        for example in tf.python_io.tf_record_iterator(input_file):
            count += 1
        num_records = count

    def gen():
        itr = tf.python_io.tf_record_iterator(input_file)
        for _, example in enumerate(tqdm(itr, total=num_records)):
            example_proto = tf.train.Example()
            example_proto.ParseFromString(example)
            im_encoded = example_proto.features.feature['image/encoded'].bytes_list.value[0]
            im = cv2.imdecode(np.frombuffer(im_encoded, dtype=np.uint8),
                              flags=cv2.IMREAD_UNCHANGED)
            if len(im.shape) > 2:
                im = im[:, :, ::-1]
            if callable(convert_fn):
                im = convert_fn(im)
            label = example_proto.features.feature['image/class/label'].int64_list.value[0]
            yield im, label

    return num_records, gen


##
def tf_parse_cls_example(serialized_example, convert_fn=None):
    """
    텐서플로우 tf.data API 나, 기타 그래프 빌드시 사용할 수 있는
    텐서플로우 버젼의 tf.train.Example() 레코드 파서.
    
    Usage:
        dataset = tf.data.TFRecordDataset(tfr_output_file)
        dataset = dataset.map(lambda x: tf_parse_cls_example(x))
        itr = dataset.make_one_shot_iterator().get_next()
        for i in range(990):
            x2_test[i], y2_test[i] = sess.run(itr)

    """
    feature = {'image/encoded': tf.FixedLenFeature([], tf.string),
               'image/class/label': tf.FixedLenFeature([], tf.int64)}
    example = tf.parse_single_example(serialized_example, feature)
    image = tf.image.decode_image(example['image/encoded'], dtype=tf.uint8)
    if callable(convert_fn):
        image = convert_fn(image)
    label = tf.cast(example['image/class/label'], tf.int32)
    return image, label


# %%
# coding: utf-8
from glob import glob
from tqdm import tqdm_notebook as tqdm
import numpy as np

## train용 tfrecord 파일은 반종옥 PD 가 준비한 train_data.npz 로 부터 생성
## 3188개 정상 이미지 데이터, shape=(3188, 256, 256, 3)
train_data_file = '/G/Train_ImageSet/skc_cathod_train_0311/train_data.npz'
train_data = np.load(train_data_file)

x_train = train_data['x_train']
y_train = train_data['y_train'].astype(int)

# %%
##
# np.load() 로 읽어온 x_train 일부 내용 확인
# %matplotlib inline
import matplotlib.pyplot as plt

fig, axs = plt.subplots(4,4,figsize=(8.5,8.5))
for i,im in enumerate(x_train[-16:]):
    r, c = i // 4, i % 4
    ax = axs[r,c]
    ax.imshow(im)

# %%
##
# write_image_to_cls_tfrecords() 를 이용해서 tfrecord 파일 생성
train_tfr = 'cathod_20190619_train.tfrecord'
train_labels = [0]*len(x_train)  # train 셋은 모두 정상 (0) 값으로 레이블
write_image_to_cls_tfrecords(x_train, train_labels, output_file=train_tfr)

# %%
## test용 tfrecord 파일은 skc_cathod_train_0311/test_data/ 아래의
## 파일들로 부터 생성
test_ok_files = glob('/G/Train_ImageSet/skc_cathod_train_0311/test_data/ok/*.jpg')
test_notok_files = glob('/G/Train_ImageSet/skc_cathod_train_0311/test_data/notok/*.jpg')
print('test_ok_files:',len(test_ok_files),'test_notok_files:',len(test_notok_files))

test_files = test_ok_files + test_notok_files
test_labels = [0] * len(test_ok_files) + [1] * len(test_notok_files)

# %%
##
# test_files 일부 내용 확인
# %matplotlib inline
import matplotlib.pyplot as plt

fig, axs = plt.subplots(4,4,figsize=(8.5,8.5))
for i,im_name in enumerate(test_files[-16:]):
    r, c = i // 4, i % 4
    ax = axs[r,c]
    ax.imshow(cv2.imread(im_name,cv2.IMREAD_UNCHANGED)[:,:,::-1])

# %%
##
# test 용 jpg/png 파일들로 부터 tfrecord 파일 생성
##
# write_image_to_cls_tfrecords() 의 images 항목은 iterable 을 지원하므로
# file을 읽어오는 generator 함수 _readfiles() 을 선언하여 사용가능
##
# test 파일들은 256x256 크기로 crop-and-resize 하지 않은 파일들이므로
# write_image_to_cls_tfrecords() 호출시 변환함수 convert_fn 을 지정하여
# 변환하도록 함

test_tfr = 'cathod_20190619_test.tfrecord'

_image_shape = (224, 224, 3) # 순서: (h, w, c)
_crop_size = 480

def _readfiles(files, tqdm=lambda x: x):
    for i in tqdm(range(len(files))):
        im_name = files[i]
        im = cv2.imread(im_name, cv2.IMREAD_UNCHANGED)
        if len(im.shape) > 2:
            im = im[:, :, ::-1]
        yield im


def _convert(im, filename=None):
    h, w = im.shape[:2]
    # 먼저, 필요시 _crop_size x _crop_size 크기로 crop
    if h > _crop_size or w > _crop_size:
        xmin, ymin = (w-_crop_size)//2, (h-_crop_size)//2
        im = im[ymin:ymin+_crop_size, xmin:xmin+_crop_size]
    # (필요시) _image_shape 에서 지정한 크기로 resize
    h, w = im.shape[:2]
    if h != _image_shape[1] or w != _image_shape[0]:
        im = cv2.resize(im, _image_shape[:2], interpolation=cv2.INTER_AREA)
    return im


image_itr = _readfiles(test_files)

write_image_to_cls_tfrecords(image_itr, test_labels, output_file=test_tfr,
                             convert_fn=_convert,
                             num_records=len(test_files))


# %%
##
import numpy as np
import cv2
from tqdm import tqdm_notebook as tqdm


cnt, gen = cls_examples_iterator(test_tfr)

x_test = np.zeros((cnt, _image_shape[1], _image_shape[0], _image_shape[2]),
                  dtype=np.uint8)
y_test = np.zeros(cnt, dtype=int)

for i, (im, lbl) in enumerate(gen()):
    x_test[i] = im
    y_test[i] = lbl

# %%
##
## tfrecord 로 부터 읽어온 x_test 일부내용 확인
# %matplotlib inline
import matplotlib.pyplot as plt

fig, axs = plt.subplots(4,4,figsize=(8.5,8.5))
for i,im in enumerate(x_test[-16:]):
    r, c = i // 4, i % 4
    ax = axs[r,c]
    ax.imshow(im)

# %%
##
## tf_parse_example() 과 tf.data 의 API 를 이용해서
## tensorflow 그래프에서 test_trf 파일의 이미지를 직접 읽어가는 방법 시험
##
cnt = 990
x2_test = np.zeros((cnt, _image_shape[1], _image_shape[0], _image_shape[2]),
                  dtype=np.uint8)
y2_test = np.zeros(cnt, dtype=int)

sess = tf.Session()

dataset = tf.data.TFRecordDataset(test_tfr)
dataset = dataset.map(lambda x: tf_parse_cls_example(x))
itr = dataset.make_one_shot_iterator().get_next()
for i in tqdm(range(990)):
    try:
        x2_test[i], y2_test[i] = sess.run(itr)
    except StopIteration:
        break

# %%
## 
## tf.data API 와 tf_parse_cls_example() 를 이용해서
## tfrecord 로 부터 읽어온 x2_test 일부내용 확인
# %matplotlib inline
import matplotlib.pyplot as plt

fig, axs = plt.subplots(4,4,figsize=(8.5,8.5))
for i,im in enumerate(x2_test[-16:]):
    r, c = i // 4, i % 4
    ax = axs[r,c]
    ax.imshow(im)

# %%


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
