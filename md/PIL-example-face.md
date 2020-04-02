---
title: PIL example face (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'face'


Modules used in program: 
* `import PIL`
* `import serial`
* `import numpy as np`
* `import pickle`
* `import cv2`

## python face

Python PIL example: face

```python
import cv2
import pickle
import numpy as np
import serial
import PIL
ser = serial.Serial('COM8', 9600, timeout=0)

face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')
cap = cv2.VideoCapture(0)
recognizer = cv2.face.LBPHFaceRecognizer_create()
recognizer.read("trainner.yml")
labels = {"person_name": 1}
with open("labels.pickle", 'rb') as f:
    og_labels = pickle.load(f)
    labels = {v:k for k,v in og_labels.items()}


while (True):
    #Capture
    ret, frame = cap.read()
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    faces = face_cascade.detectMultiScale(gray, scaleFactor=1.5, minNeighbors=5)
    for (x, y, w, h) in faces:
        #print(x,y,w,h)
        roi_gray = gray[y:y+h, x:x+w] #ycord
        roi_color = frame[y:y+h, x:x+w]
        # recognize? deep learned model predict keras tensorflor pytorch
        id_, conf = recognizer.predict(roi_gray)
        if conf>=70 and conf <=85:
            ser.write(b'1')                        #-------------------------------------serial
            #print(id_)
            print(labels[id_])
            font = cv2.FONT_HERSHEY_SIMPLEX
            name = labels[id_]
            color = (255 ,255 ,0)
            stroke =2
            cv2.putText(frame, name,(x,y), font, 1, color, stroke, cv2.LINE_AA)
        else:
            ser.write(b'0')
        #img_item = "my=image.jpg"
        #cv2.imwrite(img_item, roi_gray)

        color = (0, 255 ,0)
        stroke = 2
        end_color_x = x + w
        end_color_y = y + h
        cv2.rectangle(frame, (x, y), (end_color_x, end_color_y),color, stroke)
        ser.write(b'0')

    #Display
    cv2.imshow('frame', frame)
    if cv2.waitKey(20) & 0xFF == ord('q'):
        break


cap.release()
cv2.destroyAllWindows()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
