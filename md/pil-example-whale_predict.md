---
title: pil example whale predict (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'whale predict'

Functions in program: 
* `def ImportImage( filename):`
* `def ImportImage( filename):`
* `def extraction_Xception(file_paths):`
* `def paths_to_tensor(img_paths):`
* `def path_to_tensor(img_path):`

Modules used in program: 
* `import warnings`
* `import os`
* `import datetime as dt`
* `import cv2`
* `import matplotlib.pylab as plt`
* `import seaborn as sns`
* `import cv2`
* `import matplotlib.image as mpimg`
* `import matplotlib.pyplot as plt`
* `import h5py`
* `import numpy as np`
* `import pandas as pd`

## python whale predict

Python pil example: whale predict

```python
#Import Modules
%pylab inline
import pandas as pd
from sklearn.datasets import load_files       
from keras.utils import np_utils
import numpy as np
from glob import glob
from sklearn.model_selection import train_test_split
from keras.layers import Conv2D, MaxPooling2D, GlobalAveragePooling2D
from keras.layers import Dropout, Flatten, Dense, Activation
from keras.models import Sequential
from keras.layers.normalization import BatchNormalization
from keras.preprocessing import image                  
from tqdm import tqdm
from keras.preprocessing.image import ImageDataGenerator
from keras.callbacks import EarlyStopping, ModelCheckpoint, ReduceLROnPlateau, TensorBoard
from keras.callbacks import ModelCheckpoint
import h5py
from PIL import ImageFile                            
ImageFile.LOAD_TRUNCATED_IMAGES = True    
from keras.applications.resnet50 import ResNet50
from keras.applications.resnet50 import preprocess_input as preprocess_input_resnet50
from keras.callbacks import ModelCheckpoint  
from keras.layers import Conv2D, MaxPooling2D, GlobalAveragePooling2D
from keras.layers import Dropout, Flatten, Dense, Activation
from keras.optimizers import Adam
from keras.applications.xception import Xception
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
from sklearn.preprocessing import StandardScaler, OneHotEncoder, LabelEncoder
import cv2
import seaborn as sns
from PIL import Image
import matplotlib.pylab as plt
import cv2
from keras.models import Sequential
from keras.layers import Dense,Dropout,Conv2D,MaxPooling2D,Flatten
from keras.applications.xception import Xception
from keras.applications.inception_v3 import InceptionV3
from keras.preprocessing.image import img_to_array
from keras.applications.xception import preprocess_input as preprocess_input_xception
import datetime as dt
from IPython.display import display
import os
from os.path import split
import warnings

print('imported ...')


# Example: load a DSS dataset as a Pandas dataframe
train_images = dataiku.Folder("train-images")
test_images = dataiku.Folder("test-images")

width = 150
height = 150
channels = 3
batch_size = 16
whale_labels = dataiku.Dataset('y_train')
whale_labels = whale_labels.get_dataframe()
df = whale_labels
length=(len(whale_labels))

file=[]
PATH='/whaledata/train2/'
for row in range(len(whale_labels)):
    file.append(PATH+str(whale_labels['Image'][row]))

lbe = LabelEncoder()
yl = df['Id'].values

yl = lbe.fit_transform(yl)
onhe = OneHotEncoder()

yl = onhe.fit_transform(yl.reshape(-1,1))
img_array = np.zeros(shape=(length,width,width,3))
label_array = yl.toarray()

#test_1 are the validation files
train_files, test_1_files, train_targets, test_1_targets = train_test_split(file, label_array, test_size=0.2, random_state=1)

print('There are %d train images' % len(train_files))

def path_to_tensor(img_path):
    #loads image as PIL type
    img = image.load_img(img_path, target_size=(224, 224))
    #Convert PIL type to 3D tensor with shape (224, 224, 3)
    x = image.img_to_array(img)
    #Convert 3D tensor to 4D tensor with shape (1, 224, 224, 3)
    return np.expand_dims(x, axis=0)

def paths_to_tensor(img_paths):
    list_of_tensors = [path_to_tensor(img_path) for img_path in tqdm(img_paths)]
    return np.vstack(list_of_tensors)

#Pre-process the data for Keras
train_tensors = paths_to_tensor(train_files).astype('float32')/255
test_tensors = paths_to_tensor(test_1_files).astype('float32')/255

#Create augmented image generator and fit data
datagen = ImageDataGenerator(width_shift_range=0.2, height_shift_range=0.2, rotation_range = 20, horizontal_flip=True)
datagen.fit(train_tensors)

def extraction_Xception(file_paths):
    tensors = paths_to_tensor(file_paths).astype('float32')
    preprocessed_input = preprocess_input_xception(tensors)
    return Xception(weights='imagenet', include_top=False).predict(preprocessed_input, batch_size=32)

train_Xception = extraction_Xception(train_files)

Xception_model=Sequential()
Xception_model.add(GlobalAveragePooling2D(input_shape=train_Xception.shape[1:]))
Xception_model.add(Dense(4251, activation='softmax'))
Xception_model.summary()

Xception_model.compile(loss='categorical_crossentropy', optimizer=Adam(lr=1e-3), metrics=['accuracy'])
tb = TensorBoard(log_dir='./logs', histogram_freq=0, batch_size=32, write_graph=True, write_grads=False, write_images=False, embeddings_freq=0, embeddings_layer_names=None, embeddings_metadata=None, embeddings_data=None)

epochs = 20
batch_size = 40

history = Xception_model.fit(train_Xception, train_targets,
          epochs=epochs, validation_split = 0.2, batch_size=batch_size, callbacks=[tb,
            EarlyStopping(monitor='val_acc', patience=9, min_delta=0.005, verbose=1),
            ReduceLROnPlateau(monitor='val_acc', patience=3, min_delta=0.005, factor=0.25, min_lr=0.002, verbose=1)
        ], verbose=1,shuffle=True)

plt.plot(history.history['val_acc'], 'r--', linewidth=4.0)
plt.title('Validation Accuracy vs Number of Epochs')
plt.ylabel('Validation Accuracy')
plt.xlabel('Epoch')
plt.show()

train_images = glob("/Desktop/train2/*jpg")
test_images = glob("/Desktop/test2/*jpg")
df = pd.read_csv("/Desktop/train.csv")

df["Image"] = df["Image"].map( lambda x : "/Desktop/train2/"+x)
ImageToLabelDict = dict( zip( df["Image"], df["Id"]))

class LabelOneHotEncoder():
    def __init__(self):
        self.ohe = OneHotEncoder()
        self.le = LabelEncoder()
    def fit_transform(self, x):
        features = self.le.fit_transform( x)
        return self.ohe.fit_transform( features.reshape(-1,1))
    def transform( self, x):
        return self.ohe.transform( self.la.transform( x.reshape(-1,1)))
    def inverse_tranform( self, x):
        return self.le.inverse_transform( self.ohe.inverse_tranform( x))
    def inverse_labels( self, x):
        return self.le.inverse_transform( x)

y = list(map(ImageToLabelDict.get, train_images))
lohe = LabelOneHotEncoder()
y_cat = lohe.fit_transform(y)

def ImportImage( filename):
    img = Image.open(filename).convert("LA").resize( (SIZE,SIZE))
    return np.array(img)[:,:,0]
SIZE=64

image_gen = ImageDataGenerator(
    rescale=1./255,
    rotation_range=15,
    width_shift_range=.15,
    height_shift_range=.15,
    horizontal_flip=True)

with open("sample_submission_SEARCH.csv","w") as f:
    with warnings.catch_warnings():
        f.write("Image,Id\n")
        warnings.filterwarnings("ignore",category=DeprecationWarning)
        for image in range(15610):
            img = test_Xception[image]
            y = Xception_model.predict_proba(np.expand_dims(img, axis=0))
            predicted_args = np.argsort(y)[0][::-1][:5]
            predicted_tags = lohe.inverse_labels( predicted_args)
            image = split(test_images[image])[-1]
            predicted_tags = " ".join( predicted_tags)
            f.write("%s,%s\n" %(image, predicted_tags))           
            
lenght_test=pd.read_csv('sample_submission_SEARCH.csv')

def ImportImage( filename):
    img = Image.open(filename).convert("LA").resize( (SIZE,SIZE))
    return np.array(img)[:,:,0]
SIZE=64

image_gen = ImageDataGenerator(
    rescale=1./255,
    rotation_range=15,
    width_shift_range=.15,
    height_shift_range=.15,
    horizontal_flip=True)

with open("submission.csv","w") as f:
    with warnings.catch_warnings():
        f.write("Image,Id\n")
        warnings.filterwarnings("ignore",category=DeprecationWarning)
        for image in range(15610):
            img = test_Xception[image]
            y = Xception_model.predict_proba(np.expand_dims(img, axis=0))
            predicted_args = np.argsort(y)[0][::-1][:5]
            predicted_tags = lohe.inverse_labels( predicted_args)
            image = split(test_images[image])[-1]
            predicted_tags = " ".join( predicted_tags)
            f.write("%s,%s\n" %(image, predicted_tags))

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
