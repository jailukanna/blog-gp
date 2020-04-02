---
title: pil example load image data numpy (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'load image data numpy'


Modules used in program: 
* `import numpy as np`
* `import keras`
* `import PIL`

## python load image data numpy

Python pil example: load image data numpy

```python
#import all dependencies
import PIL
import keras
import numpy as np
from keras.preprocessing.image import ImageDataGenerator, array_to_img, img_to_array, load_img


print(("\n"))
print("Your backend is ----------------------------------------------------------------->",keras.backend.backend())

#Path of image to augment 
image_file_path = "/train/dog/dog.01.jpg"

# Loading the image 
#install pil library using pip install Pillow
img = load_img(image_file_path) 

#Convert image to 3D array of shape (image height,image width,number of channels)
image_array = img_to_array(img) 
print("Shape of input image before reshape -------------------------------------------------->",image_array.shape)

#reshape the image_array into a size (1,image height,image width,number of channels)
#Here 1 stands for single input image.
x = image_array.reshape((1,) + image_array.shape)
print("The shape of input image after reshape ----------------------------------------------->",x.shape)

print(("\n"))
print("1 in the shape, is the number of image to make x array compatible to be used as an input in the image augmenter function where the input size must always be a Numpy array of rank 4 or a tuple , followed by image height and width and then the number of channels,")
print("This data format is applied only if your backend is tensorflow.")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
