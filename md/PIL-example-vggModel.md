---
title: PIL example vggModel (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'vggModel'

Functions in program: 
* `def predictPicture(picturePath):`

Modules used in program: 
* `import numpy as np`
* `import os`
* `import sys`

## python vggModel

Python PIL example: vggModel

```python
import sys
import os
import numpy as np
from PIL import Image as pil_image
from keras.applications.vgg16 import VGG16, preprocess_input, decode_predictions


def predictPicture(picturePath):
     mod = VGG16() #Calling VGG16 Model
     img = pil_image.open(picturePath) #loading picture
     img = img.resize((224, 224)) #Resize
     dataimg = np.float64(np.array(img)) #Encode
     dataimg = np.reshape(dataimg,(1,224,224,3)) #Encode
     dataimg = preprocess_input(dataimg) #Encode
     pred = mod.predict(dataimg) #get predicts
     predlabel = decode_predictions(pred) #apply imagenet labels
     predlabel = predlabel[0][0] #get the first prediction
     return(predlabel[1] + "-" + str(predlabel[2]*100)) 

predictedResult = predictPicture(str(sys.argv[1]))
print(predictedResult)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
