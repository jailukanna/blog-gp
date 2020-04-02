---
title: PIL example find facial features in picture (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'find facial features in picture'


Modules used in program: 
* `import face_recognition`

## python find facial features in picture

Python PIL example: find facial features in picture

```python
from PIL import Image, ImageDraw
import face_recognition

image_file = 'obama.jpg' 

# Load the jpg file into a numpy array
image = face_recognition.load_image_file(image_file)

# Find all facial features in all the faces in the image
face_landmarks_list = face_recognition.face_landmarks(image)

print("I found {} face(s) in this photograph.".format(len(face_landmarks_list)))

pil_image = Image.fromarray(image)
d = ImageDraw.Draw(pil_image)

for face_landmarks in face_landmarks_list:
    # Print the location of each facial feature in this image
    facial_features = [
        'chin',
        'left_eyebrow',
        'right_eyebrow',
        'nose_bridge',
        'nose_tip',
        'left_eye',
        'right_eye',
        'top_lip',
        'bottom_lip'
    ]

    for facial_feature in facial_features:
        print("The {} in this face has the following points: {}".format(facial_feature, face_landmarks[facial_feature]))

    # Let's trace out each facial feature in the image with a line!
    for facial_feature in facial_features:
        d.line(face_landmarks[facial_feature], width=5)

# Display drawed image
pil_image.show()
#　pil_image.save('test.png')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
