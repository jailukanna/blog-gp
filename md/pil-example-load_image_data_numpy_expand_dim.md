---
title: pil example load image data numpy expand dim (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'load image data numpy expand dim'


Modules used in program: 
* `import keras`
* `import numpy as np`
* `import PIL `

## python load image data numpy expand dim

Python pil example: load image data numpy expand dim

```python
#import all dependencies
import PIL 
from keras.preprocessing.image import ImageDataGenerator, array_to_img, img_to_array, load_img
import numpy as np
import keras

print(("\n"))
print("Your backend is ----------------------------------------------------------------->",keras.backend.backend())

#Path of image to augment 
image_file_path = "/home/arya/workspace/LandT/dog.66.jpg"

# Loading the image 
#install pil library using pip install Pillow
img = load_img(image_file_path) 

#Convert image to 3D array of shape (image height,image width,number of channels)
image_array = img_to_array(img) 
print("Shape of image before reshape -------------------------------------------------->",image_array.shape)

#reshape the image_array into a size (1,image height,image width,number of channels)
#np.expand_dims adds an axis to the data. This is another way of writing the above code.
x = np.expand_dims(image_array,axis = 0)

print("The shape of image after reshape ----------------------------------------------->",x.shape)

print(("\n"))
print("1 is the number of image, followed by image height and width and then the number of channels,")
print("This data format is applied only if your backend is tensorflow.")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
