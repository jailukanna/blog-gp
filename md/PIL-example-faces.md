---
title: PIL example faces (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'faces'

Functions in program: 
* `def get_images_and_labels(path):`

Modules used in program: 
* `import time`
* `import numpy as np`
* `import cv2, os`

## python faces

Python PIL example: faces

```python
#!/usr/bin/python

# Import the required modules
import cv2, os
import numpy as np
from PIL import Image
import time
# For face detection we will use the Haar Cascade provided by OpenCV.
cascadePath = "haarcascade_frontalface_default.xml"
faceCascade = cv2.CascadeClassifier(cascadePath)

# For face recognition we will the the LBPH Face Recognizer 
recognizer = cv2.createLBPHFaceRecognizer()

def get_images_and_labels(path):
    # Append all the absolute image paths in a list image_paths
    # We will not read the image with the .sad extension in the training set
    # Rather, we will use them to test our accuracy of the training
    image_paths = [os.path.join(path, f) for f in os.listdir(path) ]
    # images will contains face images
    images = []
    # labels will contains the label that is assigned to the image
    labels = []
    print(image_paths)
    for image_path in image_paths:

        # Read the image and convert to grayscale
        image_pil = Image.open(image_path).convert('L')
        # Convert the image format into numpy array
        image = np.array(image_pil, 'uint8')
        cv2.imshow("Adding faces to traning set...", image)

        # Get the label of the image
        nbr = int(os.path.split(image_path)[1].split(".")[0].replace("subject", ""))
        # Detect the face in the image
        faces = faceCascade.detectMultiScale(image, 
            scaleFactor=1.2,
            minNeighbors=4, 
            minSize=(90, 90),
            flags=cv2.CASCADE_SCALE_IMAGE
            )
        # If face is detected, append the face to images and the label to labels
        for (x, y, w, h) in faces:
            images.append(image[y: y + h, x: x + w])
            labels.append(nbr)
            cv2.imshow("Adding faces to traning set...", image[y: y + h, x: x + w])
            # cv2.imwrite("face-" + str(x) + ".jpg", image[y: y + h, x: x + w])
            # cv2.waitKey(1000)

    # return the images list and labels list
    return images, labels

# Path to the Yale Dataset
path = './yalefaces'
path2 = './mias'
# Call the get_images_and_labels function and get the face images and the 
# corresponding labels
images, labels = get_images_and_labels(path)
cv2.destroyAllWindows()

# Perform the tranining
recognizer.train(images, np.array(labels))

# Append the images with the extension .sad into image_paths
image_paths = [os.path.join(path2, f) for f in os.listdir(path2)]
for image_path in image_paths:

# cap = cv2.VideoCapture(0)
# while(True):
    # ret, img = cap.read()
    image = cv2.imread(image_path)
    # print(image_path)
    image_gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
    # cv2.imshow("Recognizing Face", img)

    # faces = faceCascade.detectMultiScale(
    # image_gray,
    # scaleFactor=1.3,
    # minNeighbors=5,
    # minSize=(30, 30),
    # flags = cv2.cv.CV_HAAR_SCALE_IMAGE
    # )
    image = np.array(image, 'uint8')
    cv2.imshow("NO FACE", image)

    faces = faceCascade.detectMultiScale(image, 
            scaleFactor=1.2,
            minNeighbors=4, 
            minSize=(90, 90),
            flags=cv2.CASCADE_SCALE_IMAGE
            )
    print("FACES>>>>", len(faces))
    if len(faces)>0:
        # predict_image_pil = img.convert('L')
        predict_image_pil = Image.fromarray(image_gray)
        predict_image = np.array(predict_image_pil, 'uint8')

        for (x, y, w, h) in faces:
            nbr_predicted, conf = recognizer.predict(predict_image[y: y + h, x: x + w])
            print(image_path, nbr_predicted, conf)
            # cv2.imshow(str(image_path), predict_image[y: y + h, x: x + w])
            if conf < 100:
                cv2.imshow(str(nbr_predicted), predict_image[y: y + h, x: x + w])

            # nbr_actual = int(os.path.split(image_path)[1].split(".")[0].replace("subject", ""))
            # if nbr_actual == nbr_predicted:
            #     print("{} is Correctly Recognized with confidence {}".format(nbr_actual, conf))
            # else:
            #     print("{} is Incorrect Recognized as {}".format(nbr_actual, nbr_predicted))
            # cv2.imshow("Recognizing Face {}".format(x), predict_image[y: y + h, x: x + w])
            cv2.waitKey(1000)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
