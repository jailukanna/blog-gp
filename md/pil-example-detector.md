---
title: pil example detector (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'detector'


Modules used in program: 
* `import sys`
* `import pickle`
* `import numpy as np`
* `import cv2,os`

## python detector

Python pil example: detector

```python
# -*- coding: utf-8 -*-
import cv2,os
import numpy as np
from PIL import Image 
import pickle
import sys

recognizer = cv2.face.LBPHFaceRecognizer_create()
recognizer.read('trainner/trainner.yml')
cascadePath = (sys.path[0] + '/haarcascade_frontalface_default.xml')
faceCascade = cv2.CascadeClassifier(cascadePath);

cam = cv2.VideoCapture(0)
fontFace = cv2.FONT_HERSHEY_SIMPLEX
fontScale = 1
fontColor = (255, 255, 255)

while True:
    ret, im =cam.read()
    gray=cv2.cvtColor(im, cv2.COLOR_BGR2GRAY)
    faces=faceCascade.detectMultiScale(gray, scaleFactor=1.2, minNeighbors=5, minSize=(100, 100), flags=cv2.CASCADE_SCALE_IMAGE)
    for(x,y,w,h) in faces:
        nbr_predicted, conf = recognizer.predict(gray[y:y+h,x:x+w])
        cv2.rectangle(im,(x-50,y-50),(x+w+50,y+h+50),(225,0,0),2)
        
        if(nbr_predicted==7):
             nbr_predicted='Obama'
        elif(nbr_predicted==2):
             nbr_predicted='Anirban'
        elif(nbr_predicted==123):
             nbr_predicted='MAYCON'

        cv2.putText(im, str(nbr_predicted) + "--" + str(conf), (x, y+h), fontFace, fontScale, fontColor) #Draw the text
        cv2.imshow('im', im)
        cv2.waitKey(10)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
