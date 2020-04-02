---
title: pil example Face Det (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Face Det'

Functions in program: 
* `def Moveing():`
* `def Tracking():`

Modules used in program: 
* `import time`
* `import sys, os`
* `import cv2`
* `import numpy as np`

## python Face Det

Python pil example: Face Det

```python
import numpy as np
import cv2
import sys, os
from glob import glob
from picamera.array import PiRGBArray
from picamera import PiCamera
import time
from PIL import Image

#setup cam variables
FRAME_W=400
FRAME_H=240
FACE_WIDTH  = 368
FACE_HEIGHT = 448



# initialize the camera and grab a reference to the raw camera capture
camera = PiCamera()
camera.resolution = (FRAME_W, FRAME_H)#faster processing set to 160, 120
camera.framerate = 40 #max frame rate of 90 frames per second
rawCapture = PiRGBArray(camera, size=(FRAME_W, FRAME_H))#faster processing set to 160, 120
 
# allow the camera to warmup
time.sleep(0.1)

face_cascade1 = cv2.CascadeClassifier('face.xml')#lbpcascade_profileface.xml
face_cascade2 = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')#lbpcascade_frontalface.xml

#setup position variables
Position_x = 0;
Position_y = 0;
Position_z = 0;
facecount=0;

#setup servo varables
stepsize = 5
servoposX=0;
servoposY=0;
servoposZ=0;
servo0 = 0
servo1 = 0
servo2 = 0
servomin=0
servomax=200    
def Tracking():
    # capture frames from the camera
    for frame in camera.capture_continuous(rawCapture, format="bgr", use_video_port=True):
        x1=0;
        y1=0;
        w1=0;
        h1=0;
        x2=0;
        y2=0;
        w2=0;
        h2=0;
        fn=0;
        # Capture frame-by-frame
        facecount=0;
        # grab the raw NumPy array representing the image, then initialize the timestamp
        # and occupied/unoccupied text
        frame = frame.array       
        
        # Our operations on the frame come here
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
     
        # detect faces
        frontfaces = face_cascade1.detectMultiScale(gray,1.3,5)
        sidefaces = face_cascade2.detectMultiScale(gray,1.3,5)
        for (x1, y1, w1, h1) in frontfaces:
            facecount=1;
            sub_face=gray[y1:y1+h1, x1:x1+w1]
            resize_image = cv2.resize(sub_face,(FACE_WIDTH, FACE_HEIGHT),interpolation=cv2.INTER_LANCZOS4)
            cv2.imwrite("subject66.stranger.png", resize_image)
            #cv2.rectangle(gray, (x1, y1), (x1+w1, y1+h1), (255, 0, 255), 2)
            
        if facecount == 0:
            for (x2, y2, w2, h2) in sidefaces:
                facecount=1;
                #cv2.rectangle(gray, (x2, y2), (x2+w2, y2+h2), (255, 0, 255), 2)

        # Get the center of the face
        x = ((((x1+x2)/2) + ((w1+w2)/2)) /2)
        y = ((((y1+y2)/2) + ((h1+h2)/2)) /2)
        z = ((((w1+w2)/2) + ((h1+h2)/2)) /2)
        # Correct relative to center of image
        Position_x  = float(((FRAME_W/2)-x)-80)
        Position_y  = float((FRAME_H/2)-y)
        Position_z  = float(z)
        
        cv2.putText(frame,'cam_pan='+ str(Position_x),(1,50), cv2.FONT_ITALIC, 0.5,(0,0,255))
        cv2.putText(frame,'cam_tilt='+ str(Position_y),(1,150), cv2.FONT_ITALIC, 0.5,(0,0,255))
        #cv2.putText(frame,'cam_focus='+ str(Position_z),(1,200), cv2.FONT_ITALIC, 0.5,(0,0,255))
        cv2.putText(frame, 'Face(s) found = '+ str(facecount),(1,200), cv2.FONT_ITALIC, 0.5,(0,0,255))

        cv2.putText(frame,'Servo0='+ str(servo0),(1,350), cv2.FONT_ITALIC, 0.5,(0,0,255))
        cv2.putText(frame,'Servo1='+ str(servo1),(1,400), cv2.FONT_ITALIC, 0.5,(0,0,255))
        #cv2.putText(frame,'Servo2='+ str(servo2),(1,450), cv2.FONT_ITALIC, 0.5,(0,0,255))
        
        #X axis line grid
        #cv2.line(frame,((FRAME_H/2),0),(FRAME_H/2, FRAME_W),(255,255,255),1)
        #cv2.line(frame,((FRAME_H/4),0),(FRAME_H/4, FRAME_W),(255,255,255),1)
        #cv2.line(frame,((FRAME_H*3/4),0),((FRAME_H*3/4), FRAME_W),(255,255,255),1)
        #Y axis line grid
        #cv2.line(frame,(0,(FRAME_W/2)),(FRAME_H, (FRAME_W/2)),(255,255,255),1)
        #cv2.line(frame,(0,(FRAME_W/4)),(FRAME_H, (FRAME_W/4)),(255,255,255),1)
        #cv2.line(frame,(0,(FRAME_W*3/4)),(FRAME_H, (FRAME_W*3/4)),(255,255,255),1)

        if facecount == 1:
            fn = fn + 1;
            print(('processing %s...' % fn))
        
        # Display the resulting frame
        cv2.imshow('Video', frame)
        # clear the stream in preparation for the next frame
        rawCapture.truncate(0)
        
        return Position_x, Position_y, facecount

def Moveing():
    global servo0, servo1, servo2
    ### no face detected return to home position 
    if facecount == 0:
        servo0 = 50
        servo1 = 50
        servo2 = 50

    ###face detected 
    if facecount != 0:
  def Moveing():
    global servo0, servo1
    servoposX = Position_x
    servoposY = Position_y
    ### no face detected return to home position 
    if facecount == 0:
        servo0 = 45
        servo1 = 45

    ###face detected 
    if facecount != 0:
        ### limit the range of the servo positions
        print(("posX=%d" %(servoposX)))
        print(("posY=%d" %(servoposY)))
        if servoposX > (FRAME_W/4) - 10:
            if servo0 != servomin:
                servo0 = servo0 - stepsize            
        elif servoposX < (FRAME_W/4) + 10:
            if servo0 != servomax:
                servo0 = servo0 + stepsize

        if servoposY < 80:
            if servo1 != servomin:
                servo1 = servo1 - stepsize
        elif servoposY > 80:
            if servo1 != servomax:
                servo1 = servo1 + stepsize
        else:
            servo0=servo0
            servo1=servo1
        print(("servo posX=%d" %(servo0)))
        print(("servo posY=%d" %(servo1)))

    return      
    
while(True):
    Position_x, Position_y, facecount = Tracking()
    Moveing()
    #print(("Position X=%d" %(Position_x)))
    #print(("Position Y=%d" %(Position_y)))
    #print(("Position Z=%d" %(Position_z)))
    #print(("face state=%d" %(facecount)))
    # q key shuts program down 
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 # When everything is done, release the capture
video_capture.release()
cv2.destroyAllWindows()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
