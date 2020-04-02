---
title: pil example MNIST to images (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'MNIST to images'

Functions in program: 
* `def load_mnist(path, kind='train'):`

Modules used in program: 
* `import struct`
* `import os`
* `import numpy as np`

## python MNIST to images

Python pil example: MNIST to images

```python
from PIL import Image
import numpy as np
import os
import struct


# (このファイルのpath)/imagesに画像化したMNISTを保存する

def load_mnist(path, kind='train'):
    """MNISTデータをpathからロード"""
    labels_path = os.path.join(path, '%s-labels-idx1-ubyte' % kind)
    images_path = os.path.join(path, '%s-images-idx3-ubyte' % kind)

    with open(labels_path, 'rb') as lbpath:
        magic, n = struct.unpack('>II', lbpath.read(8))
        labels = np.fromfile(lbpath, dtype=np.uint8)

    with open(images_path, 'rb') as imgpath:
        magic, num, rows, cols = struct.unpack(">IIII", imgpath.read(16))
        images = np.fromfile(imgpath, dtype=np.uint8).reshape(len(labels), 784)

    return images, labels

# バイナリファイルが置いてあるpath
X_train, y_train = load_mnist('/xxxxx/mnist', kind='train')

for (i, x) in enumerate(X_train):
    image_mat = np.array([[x[i] for j in range(3)]+[0] for i in range(784)])
    image = image_mat.reshape(28, 28, 4)

    # images/images_{正解の数字(0-9)}_{1~60000の連番}.jpg
    filename = 'images/image_' + str(y_train[i]) + '_' + str(i) + '.jpg'
    print(filename)
    pil_image = Image.fromarray(image.astype('uint8'))
    pil_image.save(filename)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
