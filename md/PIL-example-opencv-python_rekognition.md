---
title: PIL example opencv-python rekognition (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'opencv-python rekognition'


Modules used in program: 
* `import io`
* `import cv2`
* `import boto3`

## python opencv-python rekognition

Python PIL example: opencv-python rekognition

```python
# With help from https://aws.amazon.com/blogs/ai/build-your-own-face-recognition-service-using-amazon-rekognition/

frame_skip = 100 # analyze every 100 frames to cut down on Rekognition API calls

import boto3
import cv2
from PIL import Image
import io

rekog = boto3.client('rekognition')

vidcap = cv2.VideoCapture('./video_clip.mp4') # Load clip from storage. Can modify this to input from camera.
cur_frame = 0
success = True

while success:

    success, frame = vidcap.read() # get next frame from video

    if cur_frame % frame_skip == 0: # only analyze every n frames
        print('frame: {}'.format(cur_frame)) 

        pil_img = Image.fromarray(frame) # convert opencv frame (with type()==numpy) into PIL Image
        stream = io.BytesIO()
        pil_img.save(stream, format='JPEG') # convert PIL Image to Bytes
        bin_img = stream.getvalue()

        response = rekog.recognize_celebrities(Image={'Bytes': bin_img}) # call Rekognition
        if response['CelebrityFaces']: # print(celebrity name if a celebrity is detected)
            for face in response['CelebrityFaces']:
                print('Celebrity is {} with confidence of {}'.format(face['Name'], face['MatchConfidence']))

    cur_frame += 1


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
