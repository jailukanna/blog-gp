---
title: PIL example inception test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'inception test'

Functions in program: 
* `def plot_preds(image, preds):`
* `def predict(model, img, target_size):`

Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import requests`
* `import numpy as np`
* `import argparse`
* `import sys`

## python inception test

Python PIL example: inception test

```python
import sys
import argparse
import numpy as np
from PIL import Image
import requests
from io import BytesIO
import matplotlib.pyplot as plt
from PIL import Image,ImageDraw,ImageFont
from keras.preprocessing import image
from keras.models import load_model
from keras.applications.inception_v3 import preprocess_input
target_size = (229, 229) #fixed size for InceptionV3 architecture
def predict(model, img, target_size):
 """Run model prediction on image
 Args:
 model: keras model
 img: PIL format image
 target_size: (w,h) tuple
 Returns:
 list of predicted labels and their probabilities
 """
 if img.size != target_size:
 img = img.resize(target_size)
x = image.img_to_array(img)
 x = np.expand_dims(x, axis=0)
 x = preprocess_input(x)
 preds = model.predict(x)
 return preds[0]
def plot_preds(image, preds):
 """Displays image and the top-n predicted probabilities in a bar graph
 Args:
 image: PIL image
 preds: list of predicted labels and their probabilities
 """
 plt.imshow(image)
 plt.axis('off')
plt.figure()
 labels = ("daisy", "dandelion","roses","sunflower","tulips")
 plt.barh([0, 1,2,3,4], preds, alpha=0.5)
 plt.yticks([0, 1,2,3,4], labels)
 plt.xlabel('Probability')
 plt.xlim(0,1.01)
 plt.tight_layout()
 plt.show()
if __name__=="__main__":
 a = argparse.ArgumentParser()
 a.add_argument(" - image", help="path to image")
 a.add_argument(" - image_url", help="url to image")
 a.add_argument(" - model")
 args = a.parse_args()
 
 if args.image is None and args.image_url is None:
 a.print_help()
 sys.exit(1)
model = load_model(args.model)
 model.fit()
 if args.image is not None:
 labels = ("daisy", "dandelion","roses","sunflower","tulips")
 image1 = Image.open(args.image)
 preds = predict(model, image1, target_size) 
 print(preds)
 preds = preds.tolist()
 plot_preds(image1, preds)
 fonttype = ImageFont.truetype("/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf",18)
 draw = ImageDraw.Draw(image1)
 draw.text(xy=(5,5),text = str(labels[preds.index(max(preds))])+":"+str(max(preds)),fill = (255,255,255,128),font = fonttype)
 image1.show()
 image1.save((args.image).split(".")[0]+"1"+".jpg")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
