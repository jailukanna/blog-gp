---
title: PIL example Comb Face Det RecogV3 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Comb Face Det RecogV3'

Functions in program: 
* `def get_images_and_labels(path):`
* `def Moveing():`
* `def Tracking():  `

Modules used in program: 
* `import numpy as np`
* `import time`
* `import os`
* `import cv2`

## python Comb Face Det RecogV3

Python PIL example: Comb Face Det RecogV3

```python
# put your imports here
import cv2
import os
from glob import glob
from picamera.array import PiRGBArray
from picamera import PiCamera
import time
from PIL import Image
import numpy as np

# Import the required modules
print("Initialising...")

# For face detection we will use the Haar Cascade provided by OpenCV.
cascadePath = "haarcascade_frontalface_default.xml"
faceCascade = cv2.CascadeClassifier(cascadePath)

# For face recognition we will the the LBPH Face Recognizer 
recognizer = cv2.createLBPHFaceRecognizer()

yesChoice = ['yes','y']
noChoice = ['no','n']
####################


#setup cam variables
FRAME_W= 480#960
FRAME_H= 270#540

# initialize the camera and grab a reference to the raw camera capture
camera = PiCamera()
camera.resolution = (FRAME_W, FRAME_H)#faster processing set to 160, 120
camera.framerate = 60 #max frame rate of 90 frames per second
rawCapture = PiRGBArray(camera, size=(FRAME_W, FRAME_H))#faster processing set to 160, 120

# allow the camera to warmup
time.sleep(0.1)

face_cascade1 = cv2.CascadeClassifier('face.xml')#lbpcascade_profileface.xml
face_cascade2 = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')#lbpcascade_frontalface.xml
HAAR_SCALE_FACTOR  = 1.3
HAAR_MIN_NEIGHBORS = 4
HAAR_MIN_SIZE      = (30, 30)

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
servomax=90

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
      gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
      # detect faces for the side face_cascade1
      # detect faces for the side face_cascade2      
      frontfaces = face_cascade1.detectMultiScale(gray,
          scaleFactor=HAAR_SCALE_FACTOR, 
          minNeighbors=HAAR_MIN_NEIGHBORS, 
          minSize=HAAR_MIN_SIZE, 
          flags=cv2.CASCADE_SCALE_IMAGE)
      
      sidefaces = face_cascade2.detectMultiScale(gray, 
                      scaleFactor=HAAR_SCALE_FACTOR, 
                      minNeighbors=HAAR_MIN_NEIGHBORS, 
                      minSize=HAAR_MIN_SIZE, 
                      flags=cv2.CASCADE_SCALE_IMAGE)
  
      for (x1, y1, w1, h1) in frontfaces:
          facecount=1;
          cv2.rectangle(frame, (x1, y1), (x1+w1, y1+h1), (255, 0, 255), 2)
          
      if facecount == 0:
          for (x2, y2, w2, h2) in sidefaces:
              facecount=1;
              cv2.rectangle(frame, (x2, y2), (x2+w2, y2+h2), (255, 0, 255), 2)

      # Get the center of the face
      x = ((((x1+x2)/2) + ((w1+w2)/2)) /2)
      y = ((((y1+y2)/2) + ((h1+h2)/2)) /2)
      z = ((((w1+w2)/2) + ((h1+h2)/2)) /2)
      # Correct relative to center of image
      Position_x  = float(((FRAME_W/2)-x)-80)
      Position_y  = float((FRAME_H/2)-y)
      Position_z  = float(z)
      
      #cv2.putText(frame,'cam_pan='+ str(Position_x),(1,50), cv2.FONT_ITALIC, 0.5,(0,0,255))
      #cv2.putText(frame,'cam_tilt='+ str(Position_y),(1,150), cv2.FONT_ITALIC, 0.5,(0,0,255))
      #cv2.putText(frame,'cam_focus='+ str(Position_z),(1,200), cv2.FONT_ITALIC, 0.5,(0,0,255))
      #cv2.putText(frame, 'Face(s) found = '+ str(facecount),(1,200), cv2.FONT_ITALIC, 0.5,(0,0,255))

      #cv2.putText(frame,'Servo0='+ str(servo0),(1,350), cv2.FONT_ITALIC, 0.5,(0,0,255))
      #cv2.putText(frame,'Servo1='+ str(servo1),(1,400), cv2.FONT_ITALIC, 0.5,(0,0,255))
      #cv2.putText(frame,'Servo2='+ str(servo2),(1,450), cv2.FONT_ITALIC, 0.5,(0,0,255))
      
      #X axis line grid
      #cv2.line(frame,((FRAME_H/2),0),(FRAME_H/2, FRAME_W),(255,255,255),1)
      #cv2.line(frame,((FRAME_H/4),0),(FRAME_H/4, FRAME_W),(255,255,255),1)
      #cv2.line(frame,((FRAME_H*3/4),0),((FRAME_H*3/4), FRAME_W),(255,255,255),1)
      #Y axis line grid
      #cv2.line(frame,(0,(FRAME_W/2)),(FRAME_H, (FRAME_W/2)),(255,255,255),1)
      #cv2.line(frame,(0,(FRAME_W/4)),(FRAME_H, (FRAME_W/4)),(255,255,255),1)
      #cv2.line(frame,(0,(FRAME_W*3/4)),(FRAME_H, (FRAME_W*3/4)),(255,255,255),1)
      if input in yesChoice:
          if facecount == 1:
              sub_face=gray[y1:y1+h1, x1:x1+w1]
              cv2.imwrite('./detected_face/subject69.stranger.png', sub_face)
              ###############################
              print("Searching database")
              image_paths = [os.path.join(path, f) for f in os.listdir(path) ] #if f.endswith('.sad')
              for image_path in image_paths:
                  predict_image_pil = Image.open(image_path).convert('L')
                  predict_image = np.array(predict_image_pil, 'uint8')
                  faces = faceCascade.detectMultiScale(predict_image)
                  for (x, y, w, h) in faces:
                      nbr_predicted, conf = recognizer.predict(predict_image[y: y + h, x: x + w])
                      nbr_actual = int(os.path.split(image_path)[1].split(".")[0].replace("subject", ""))
                      if nbr_actual == nbr_predicted:
                          #print("{} is Correctly Recognized with confidence {}".format(nbr_actual, conf))
                          print"{} Correctly recognised as {}. Confidence = {}".format(nbr_actual, nbr_predicted, conf) 
                      else:
                          #print("{} is Incorrect Recognized as {}".format(nbr_actual, nbr_predicted))
                          print"{} Incorrectly recognised as {}. Confidence = {}".format(nbr_actual, nbr_predicted, conf)
                      if conf < 100:
                          print("Subject Identified")
                          if nbr_predicted == 16:
                              name ='Philip'
                          elif nbr_predicted == 17:
                              name ='David'
                          elif nbr_predicted == 41:
                              name ='Robert'
                          else:
                              name ='Unknown'
                      else:
                          print("Subject Unknown")
                          name ='Unknown'
                      print(name)
                      cv2.putText(predict_image[y: y + h, x: x + w],''+str(name),(10,50), cv2.FONT_ITALIC, 2, (0, 0, 255),3)
                      cv2.imshow("Recognizing Face {}", predict_image[y: y + h, x: x + w])
                      cv2.waitKey(1000)
                      ############################
                  
      cv2.imshow("Video", frame)
      # clear the stream in preparation for the next frame
      rawCapture.truncate(0)
      return Position_x, Position_y, facecount

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

#####################  

#####################
def get_images_and_labels(path):
  # Append all the absolute image paths in a list image_paths
  # We will not read the image with the .sad extension in the training set
  # Rather, we will use them to test our accuracy of the training
  image_paths = [os.path.join(path, f) for f in os.listdir(path) if not f.endswith('.sad')]
  # images will contains face images
  images = []
  # labels will contains the label that is assigned to the image
  labels = []
  for image_path in image_paths:
      # Read the image and convert to grayscale
      image_pil = Image.open(image_path).convert('L')
      # Convert the image format into numpy array
      image = np.array(image_pil, 'uint8')
      # Get the label of the image
      nbr = int(os.path.split(image_path)[1].split(".")[0].replace("subject", ""))
      # Detect the face in the image
      faces = faceCascade.detectMultiScale(image)
      # If face is detected, append the face to images and the label to labels
      for (x, y, w, h) in faces:
          print('.',)
          images.append(image[y: y + h, x: x + w])
          labels.append(nbr)
          cv2.imshow("Adding faces to traning set...", image[y: y + h, x: x + w])
          cv2.waitKey(50)
  # return the images list and labels list
  return images, labels
##############################

##############################
input = raw_input("Would you like to enable face recognition? (y/n) ").lower()
if input in yesChoice:
  print("Loading Database.")

  # Path to the Yale Dataset
  path = strpath + 'yalefaces'
  # Call the get_images_and_labels function and get the face images and the 
  # corresponding labels
  images, labels = get_images_and_labels(path)
  cv2.destroyAllWindows()

  # Perform the tranining
  print("Training...")
  recognizer.train(images, np.array(labels))
  path = strpath + 'detected_face'
############################

# main loop
while 1:
    Position_x, Position_y, facecount = Tracking()
    Moveing()

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
