---
title: PIL example VGG16 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'VGG16'


## python VGG16

Python PIL example: VGG16

```python
from keras.preprocessing import image
from keras.models import Model
from keras.applications.vgg16 import VGG16
from keras.applications.vgg16 import preprocess_input
from PIL import Image as pil_image

model_vgg16_conv = VGG16(weights='imagenet', include_top=True)
print(model_vgg16_conv.summary())
"""
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
input_1 (InputLayer)         (None, 224, 224, 3)       0         
_________________________________________________________________
block1_conv1 (Conv2D)        (None, 224, 224, 64)      1792      
_________________________________________________________________
block1_conv2 (Conv2D)        (None, 224, 224, 64)      36928     
_________________________________________________________________
block1_pool (MaxPooling2D)   (None, 112, 112, 64)      0         
_________________________________________________________________
block2_conv1 (Conv2D)        (None, 112, 112, 128)     73856     
_________________________________________________________________
block2_conv2 (Conv2D)        (None, 112, 112, 128)     147584    
_________________________________________________________________
block2_pool (MaxPooling2D)   (None, 56, 56, 128)       0         
_________________________________________________________________
block3_conv1 (Conv2D)        (None, 56, 56, 256)       295168    
_________________________________________________________________
block3_conv2 (Conv2D)        (None, 56, 56, 256)       590080    
_________________________________________________________________
block3_conv3 (Conv2D)        (None, 56, 56, 256)       590080    
_________________________________________________________________
block3_pool (MaxPooling2D)   (None, 28, 28, 256)       0         
_________________________________________________________________
block4_conv1 (Conv2D)        (None, 28, 28, 512)       1180160   
_________________________________________________________________
block4_conv2 (Conv2D)        (None, 28, 28, 512)       2359808   
_________________________________________________________________
block4_conv3 (Conv2D)        (None, 28, 28, 512)       2359808   
_________________________________________________________________
block4_pool (MaxPooling2D)   (None, 14, 14, 512)       0         
_________________________________________________________________
block5_conv1 (Conv2D)        (None, 14, 14, 512)       2359808   
_________________________________________________________________
block5_conv2 (Conv2D)        (None, 14, 14, 512)       2359808   
_________________________________________________________________
block5_conv3 (Conv2D)        (None, 14, 14, 512)       2359808   
_________________________________________________________________
block5_pool (MaxPooling2D)   (None, 7, 7, 512)         0         
_________________________________________________________________
flatten (Flatten)            (None, 25088)             0         
_________________________________________________________________
fc1 (Dense)                  (None, 4096)              102764544 
_________________________________________________________________
fc2 (Dense)                  (None, 4096)              16781312  
_________________________________________________________________
predictions (Dense)          (None, 1000)              4097000   
=================================================================
Total params: 138,357,544
Trainable params: 138,357,544
Non-trainable params: 0
_________________________________________________________________
"""

# create new model that uses the input and the last fully connected layer from vgg16
model = Model(inputs=model_vgg16_conv.input,
          outputs=model_vgg16_conv.get_layer('fc2').output)

#create placeholder for images to go through neural net
images = np.zeros(shape=(1, 224, 224, 3))

#Keras is more used to deal with PIL images 
img = pil_image.open("random_image.jpg")
if img.mode != 'RGB':
    img = img.convert('RGB')
# Model requires the input shape to be (224,224,3)    
img = img.resize((224, 224), pil_image.NEAREST)
x_raw = image.img_to_array(img)
#overwrite first instance of images placeholder with the image array
x_expand = np.expand_dims(x_raw, axis=0)
images[0, :, :, :] = x_expand

# preprocess your image to be able to enter the neural network
inputs = preprocess_input(images)
#predict image features
images_features = model.predict(inputs)
vector = images_features[0]
print(vector.shape)
# (4096,)
print(vector)
# [0.        0.        0.        ... 0.        1.9249601 0.8309637]

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
