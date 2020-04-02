---
title: PIL example TFRecord Generator (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'TFRecord Generator'

Functions in program: 
* `def make_q_list(filepathlist, filetype):`

Modules used in program: 
* `import PIL`
* `import os`
* `import numpy as np`
* `import pandas as pd`
* `import tensorflow as tf`

## python TFRecord Generator

Python PIL example: TFRecord Generator

```python
import tensorflow as tf
import pandas as pd
import numpy as np
import os
from tqdm import tqdm
from PIL import Image
from PIL import ImageFont
from PIL import ImageDraw
from PIL import Image, ImageOps
import PIL

def make_q_list(filepathlist, filetype):
    filepathlist = filepathlist
    filepaths = []
    labels = []
    for path in filepathlist:
        data_files = os.listdir(path)
        for data in data_files:
            if data.endswith(filetype):
                data_file = os.path.join(path, data)
                data_file = data_file
                data_label = os.path.basename(os.path.normpath(path))
                filepaths.append(data_file)
                labels.append(data_label)
    return filepaths, labels

class TF_Record_Generator(object):
    '''
    This will take a continuous stream of data and output it to a TFRecord File for use
    in a Tensorflow model. Currently I am uncertain if this can handle mixed data types
    in a table currently This may require some pre-proccessing to make it all either a
    float, int, or byte dtype.
    RECOMMENDATION: Save image sizes in the TFRecord file name until I figure out how
    to parse them without prior knowledge of their sizes. Also for images, may want to
    keep track of your label index.
    '''
    def __init__(self, tf_filename, file_queue_list=None, labels_list=None,
                 dtype='float', class_index=None, img_resize=None):
        self.dtype = dtype
        if self.dtype == 'float':
            self.dtype = self._float_feature
        elif self.dtype == 'int':
            self.dtype = self._int64_feature
        elif self.dtype == 'byte':
            self.dtype = self._bytes_feature
        self.labels_list = labels_list
        self.img_resize = img_resize
        self.file_queue_list = file_queue_list
        self.writer = tf.python_io.TFRecordWriter(tf_filename)
        img_ext = ['.jpg', '.png', '.bmp', '.gif']
        tbl_ext = ['.csv', '.h5']
        ext_set = set()
        for f in self.file_queue_list:
            ext_set.add(os.path.splitext(f)[1])
        ext_set = list(ext_set)
        if pd.Series(list(ext_set)).isin(tbl_ext)[0] == True:
            print('Table')
            self.table_2TFR()
        elif pd.Series(list(ext_set)).isin(img_ext)[0] == True:
            print('Image')
            #if you don't have a list of classes, make one
            if class_index is None:
                self.class_index = set()
                for l in labels_list:
                    self.class_index.add(l)
                self.class_index = list(self.class_index)
            else:
                self.class_index = class_index
            self.img_2TFR()
            print('Image Class Index:', self.class_index)

    def _int64_feature(self, value):
        return tf.train.Feature(int64_list=tf.train.Int64List(value=[value]))

    def _float_feature(self, value):
        return tf.train.Feature(float_list=tf.train.FloatList(value=value))

    def _bytes_feature(self, value):
        return tf.train.Feature(bytes_list=tf.train.BytesList(value=[value]))

    def table_2TFR(self):
        for file in tqdm(self.file_queue_list):
            if os.path.splitext(file)[1] == '.csv':
                data = pd.read_csv(file).values
            elif os.path.splitext(file)[1] == '.hdf':
                data = pd.read_hdf(file).values
            else:
                print(os.path.splitext(file)[1], 'is not supported at this time...')
                break
            for row in data:
                example = tf.train.Example(
                    features=tf.train.Features(feature={
                        'x': self.dtype(row)
                    })
                )
                self.writer.write(example.SerializeToString())

    def label_to_int(self, labels_list=None, index=None):  # labels is a list of labels
        if index is None:
            class_index = set()
            for l in labels_list:
                class_index.add(l)
                class_index = list(class_index)
        else:
            class_index = index
        int_classes = []
        for label in labels_list:
            int_classes.append(class_index.index(label))  # the class_index.index() looks values up in the list label
        int_classes = np.array(int_classes, dtype=np.uint32)
        return int_classes, class_index  #returning class index so you know what things are

    def image_resizer(self, image, size):

        basewidth = size
        img = Image.fromarray(image)
        wpercent = (basewidth / float(img.size[0]))
        hsize = int((float(img.size[1]) * float(wpercent)))
        img = img.resize((basewidth, hsize), PIL.Image.ANTIALIAS)
        #TODO: Adjust this for odd size images
        # crop the sides off to make a square image
        img = ImageOps.fit(img, (size, size), Image.ANTIALIAS)
        return np.asarray(img)

    def read_image(self, path2Img, lbl_int, size=None):
        images = []
        im = Image.open(path2Img)
        im = np.asarray(im, np.uint8)
        if size != None:
            im = self.image_resizer(im, size)
        images.append([lbl_int, im])
        images = sorted(images, key=lambda image: image[0])
        image_shape = images[0][1].shape
        images_only = [np.asarray(image[1].flatten(), np.uint8) for image in images]
        images_only = np.array(images_only)
        labels_only = [np.asarray(key[0], np.int32) for key in images]
        labels_only = np.array(labels_only)
        return images_only, labels_only, image_shape

    def img_2TFR(self):
        # take class list and convert the class name to int values
        int_classes, class_index = self.label_to_int(labels_list=self.labels_list,
                                                     index=self.class_index)
        # for every img in the queue read the image, output something to be written to TFR
        for im in tqdm(range(len(self.file_queue_list))):
            img, lbl, image_shape = self.read_image(self.file_queue_list[im],
                                       int_classes[im],
                                       size=self.img_resize)
            rows = image_shape[0]
            cols = image_shape[1]
            depth = image_shape[2]
            img = img.tostring()
            example = tf.train.Example(features=tf.train.Features(feature={
                'height': self._int64_feature(rows),
                'width': self._int64_feature(cols),
                'depth': self._int64_feature(depth),
                'label': self._int64_feature(int(lbl)),
                'image_raw': self._bytes_feature(img)}))
            self.writer.write(example.SerializeToString())

class TF_Record_Reader(object):
    ''' Currently only does shuffled batches of images, train_shuffle_batch=False for dataframes
    will result in the entire dataframe being returned unshuffled. '''
    def TFR2_img(self, tf_filepath, batch_size,
                 num_epochs=None, n_classes=None, one_hot=False, flat_imshape=None,
                 num_threads=2):
        self.tf_filepath = tf_filepath
        self.num_threads = num_threads
        self.batch_size = batch_size
        self.flat_imshape = flat_imshape
        filename_queue = tf.train.string_input_producer(
            [tf_filepath], num_epochs=num_epochs)
        self.images, self.labels = self.image_read_and_decode(filename_queue)
        if one_hot:
            self.labels = tf.one_hot(self.labels, n_classes, dtype=tf.int32)
        self.type = 'img'
        self.enqueue_many = False
        return self.shuffle_batch()

    def image_read_and_decode(self, filename_queue):
        reader = tf.TFRecordReader()
        _, serialized_example = reader.read(filename_queue)
        features = tf.parse_single_example(
            serialized_example,
            features={
                'image_raw': tf.FixedLenFeature([], tf.string),
                'label': tf.FixedLenFeature([], tf.int64),
                'height': tf.FixedLenFeature([], tf.int64),
                'width': tf.FixedLenFeature([], tf.int64),
                'depth': tf.FixedLenFeature([], tf.int64)
            })
        image = tf.decode_raw(features['image_raw'], tf.uint8)
        height = tf.cast(features['height'], tf.int32)
        width = tf.cast(features['width'], tf.int32)
        depth = tf.cast(features['depth'], tf.int32)
        image_shape = tf.pack([height, width, depth])
        #TODO: Fix this so it can get the shape of the image by itself.
        image.set_shape([self.flat_imshape])
        image = tf.cast(image, tf.float32)
        label = tf.cast(features['label'], tf.int32)
        return image, label

    def shuffle_batch(self):
        if self.type == 'img':
            data = [self.images, self.labels]
        elif self.type == 'df':
            data = [self.dataframe]
        #This is similar to the 'inputs' function in Tensorflow tutorials/code
        example_batch = tf.train.shuffle_batch(
            data, batch_size=self.batch_size, num_threads=self.num_threads,
            capacity=1000, enqueue_many=self.enqueue_many,
            # Ensures a minimum amount of shuffling of examples.
            min_after_dequeue=10, name=self.tf_filepath)
        if self.type == 'img':
            return example_batch[0], example_batch[1]
        #when running to convert to DF looks like this...
        # df = sess.run([example_batch])
        # df = pd.DataFrame(df[0])
        elif self.type == 'df':
            return example_batch

    def TFR2_df(self, tf_filepath, cols_count, batch_size=None,
                train_shuffle_batch=False, num_epochs=2, num_threads=2):

        self.train_shuffle_batch = train_shuffle_batch
        self.num_epochs = num_epochs
        self.batch_size = batch_size
        self.num_threads = num_threads
        self.tf_filepath = tf_filepath
        features = {'x': tf.FixedLenFeature([cols_count], tf.float32)}
        data = []
        for s_example in tf.python_io.tf_record_iterator(self.tf_filepath):
            example = tf.parse_single_example(s_example, features=features)
            data.append(tf.expand_dims(example['x'], 0))
        self.type = 'df'
        self.enqueue_many = True

        self.dataframe = tf.concat(0, data)
        if self.train_shuffle_batch == False:
            return self.dataframe
        else:
            return self.shuffle_batch()


if __name__ == "__main__":
    # #For Images
    ##Find some images to test this out with...
    cats = '/home/mcamp/Pictures/test/cat'
    dogs = '/home/mcamp/Pictures/test/dog'
    path_list = [cats, dogs]
    q, label = make_q_list(path_list, 'jpg')
    TF_Record_Generator('testtf.tfrecords', file_queue_list=q, labels_list=label)
    train_dir = '/home/mcamp/PycharmProjects/ACDC LSTM/'
    tf_file = 'testtf.tfrecords'
    tfr = TF_Record_Reader()
    Img_train_batch, lbl_train_batch = tfr.TFR2_img(tf_filepath=tf_file,
                                                    batch_size=2, num_epochs=None,
                                                    n_classes=2, one_hot=False,
                                                    flat_imshape=200*200*3)

    #For DataFrames
    for i in range(10):
        filename = '/home/mcamp/PycharmProjects/ACDC_VisMag_LSTM/Data/random_csv' + str(i) + '.csv'
        pd.DataFrame(np.arange(0.0, 5000.0).reshape((100, 50))).to_csv(filename)

    filepathlist = ['/home/mcamp/PycharmProjects/ACDC_VisMag_LSTM/Data/']
    q, _ = make_q_list(filepathlist, '.csv')
    tffilename = 'Demo_TFR.tfrecords'
    TF_Record_Generator(tffilename, file_queue_list=q)
    tfr = TF_Record_Reader()
    ##Return whole TFR file in one batch
    DF_train_whole = tfr.TFR2_df(tffilename, cols_count=51,
                train_shuffle_batch=False, num_epochs=2, num_threads=2)

    ##Return shuffled and batched TFR
    DF_train_batch = tfr.TFR2_df(tffilename, batch_size=3, cols_count=51,
                train_shuffle_batch=True, num_epochs=2, num_threads=2)

    with tf.Session() as sess:
        init_op = tf.group(tf.global_variables_initializer(), tf.local_variables_initializer())
        sess.run(init_op)
        coord = tf.train.Coordinator()
        threads = tf.train.start_queue_runners(coord=coord)
        Xwhole, Xbatch = sess.run([DF_train_whole, DF_train_batch])
        print('All Records Shape:', Xwhole.shape, 'Batch Records Shape:', Xbatch.shape)


    with tf.Session() as sess:
        init_op = tf.group(tf.global_variables_initializer(), tf.local_variables_initializer())
        sess.run(init_op)
        coord = tf.train.Coordinator()
        threads = tf.train.start_queue_runners(coord=coord)
        images, labels = sess.run([Img_train_batch, lbl_train_batch])
        image = np.reshape(images[0], (200,200,3))
        image = np.array(image, dtype=np.uint8)
        im = Image.fromarray(image)
        print('Int Label:', np.argmax(labels[0], axis=0))
        im.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
