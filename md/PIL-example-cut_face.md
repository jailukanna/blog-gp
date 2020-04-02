---
title: PIL example cut face (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'cut face'


Modules used in program: 
* `import sys`
* `import os`
* `import datetime`
* `import time`
* `import cv2`
* `import numpy as np`

## python cut face

Python PIL example: cut face

```python
# -*- coding: utf-8 -*-
from  __future__ import print_function
import numpy as np
from PIL import Image
import cv2
import time
import datetime
import os
import sys



class faceCropper:
    def __init__(self,facesLocationTosave="/tmp/temp/"):
        # self.face_cascade        = cv2.CascadeClassifier('./trainner/trainner.xml')
        self.face_cascade        = cv2.CascadeClassifier(sys.path[0] + '/haarcascade_frontalface_default.xml')
        self.frame               = ""
        self.FACES_LOCATION      = facesLocationTosave
        # if not os.path.isdir(self.FACES_LOCATION):
        #     os.mkdir(self.FACES_LOCATION)
 
    def saveToFile(self, img, TARGET, imgOriginalName):
        filename = imgOriginalName.split(".")[0]
        if len(filename) < 10:
            for i in range(10-len(filename)):
                filename += "0"
        filename += ".jpg"
        cv2.imwrite(TARGET + filename, img)
 
    """
    faceDetect
    Faz a deteccao da face e salva em self.FACES_LOCATION com o nome original. O nome original eh o timestamp pego com long(time.time()).
 
    if w < 868 eh um gatilho falso para permitir faces maiores. Talvez seja necessario limitar o tamanho minimo,
    dependendo da reação do faceRecognition
    """
    def faceDetect(self, img, nameToSave):
        imagePath = self.FACES_LOCATION + img

        pilImage = Image.open(imagePath).convert('L')
        imageNp = np.array(pilImage,'uint8')

        faces = self.face_cascade.detectMultiScale(imageNp, scaleFactor=1.3, minNeighbors=5, flags=cv2.CASCADE_SCALE_IMAGE,minSize=(50, 50), maxSize=None)
 
        if len(faces) > 0:
            print("Pessoa detectada! Arquivo: " + img)
        for (x, y, w, h) in faces:
            if w < 868:
                roi_gray = imageNp[y - 20:y + h + 20, x - 20:x + w + 20]
                self.saveToFile(roi_gray, '', nameToSave)
                cv2.rectangle(imageNp, (x - 10, y - 20), (x + w + 10, y + h + 10), (0, 255, 0), 2)
                print("File save path: " + nameToSave)
                print(x, y)
                print(w, h)
    
    def resize(self, target, nameImage, basewidth=200, baseheight=200):
        imagePath = self.FACES_LOCATION + nameImage
        img = Image.open(imagePath).convert('L')
            
        if img.size[0] <= img.size[1]:
            hpercent = (baseheight / float(img.size[1]))
            wsize = int((float(img.size[0]) * float(hpercent)))
            img = img.resize((wsize, baseheight), Image.ANTIALIAS)
        else:
            wpercent = (basewidth / float(img.size[0]))
            hsize = int((float(img.size[1]) * float(wpercent)))
            img = img.resize((basewidth, hsize), Image.ANTIALIAS)

        img.save(target + nameImage)
        


    def getImagesAndLabels(self, path, ids):
        imagesPath = path
        listImages = os.listdir(imagesPath)
        arrayImages = []
        arrayIds = []
        
        for image in listImages:
            pilImage = Image.open(imagesPath + image).convert('L')
            arrayImages.append(np.array(pilImage,'uint8'))
            arrayIds.append(ids)

        return arrayImages, arrayIds

    def generateTrainner(self, path, ids):
        recognizer = cv2.face.LBPHFaceRecognizer_create()

        faces, Ids = self.getImagesAndLabels(path, ids)
        recognizer.train(faces, np.array(Ids))
        recognizer.save('trainner/trainner.xml')



path = "images/"
pathFaces = "face/"
pathResize = "resize/"
listImages = os.listdir(path)

# Ira encontrar um rotos, corta a imagem, alinhas na altura dos olhos, e colocar cinza
faceCut = faceCropper(path)
for image in listImages:
    if image.find(".jpg") != -1:
        faceCut.faceDetect(image, pathFaces + image)

# não é necessário ser executado
# listImages = os.listdir(pathFaces)
# for image in listImages:
#     if image.find(".jpg") != -1:
#         faceCut.resize(pathResize, image)
        # faceCut.FACES_LOCATION = pathResize

# faceCut.generateTrainner(pathFaces, 123)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
