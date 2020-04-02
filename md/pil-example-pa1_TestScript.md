---
title: pil example pa1 TestScript (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'pa1 TestScript'


Modules used in program: 
* `import numpy as np`
* `import PCA as p`
* `import dataloader as DL`
* `import emotionScripts as em`
* `import matplotlib.pyplot as plt`

## python pa1 TestScript

Python pil example: pa1 TestScript

```python


from sklearn.model_selection import train_test_split
from sklearn.model_selection import GridSearchCV
from sklearn.datasets import fetch_lfw_people
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.decomposition import PCA
from sklearn.svm import SVC
import matplotlib.pyplot as plt
import emotionScripts as em
import dataloader as DL
import PCA as p
import numpy as np

from PIL import Image

data_img, labels = DL.load_data()
data_img = np.asarray(data_img)  # Convert from List to ND-Array

# Convert to Float and Reshape for PCA Operations
data_img = data_img.astype(float)
numImgs, height, width = data_img.shape
data_pca = data_img.reshape(numImgs, height*width)
cafe = em.DataSet(data_img, labels)

trainTotal = list(set(cafe.trainHappy + cafe.trainSad))

trainImages = np.asarray([t.image for t in trainTotal])
trainImgPCA = trainImages.reshape(len(trainTotal), height * width)

############################################################
n_components = len(trainTotal)
pca = PCA(n_components=n_components, svd_solver='randomized',
          whiten=True).fit(trainImgPCA)
eigFace, _, _ = p.PCA(trainImages, 16)
norm_eig = np.linalg.norm(eigFace, 2,axis=0)

eigFace = eigFace/norm_eig

eigFace = (eigFace.T).reshape(n_components,height,width)


eigenfaces = pca.components_.reshape((n_components, height, width))


print("Projecting the input data on the eigenfaces orthonormal basis")
I = eigenfaces[0]
I8 = (((I - I.min()) / (I.max() - I.min())) * 255.9).astype(np.uint8)

# img = Image.fromarray(I8)
DL.display_face(I8)

I = eigFace[0]
I8 = (((I - I.min()) / (I.max() - I.min())) * 255.9).astype(np.uint8)

# img = Image.fromarray(I8)

DL.display_face(I8)

print(p.test_PCA(eigFace))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
