---
title: PIL example Face Recognition (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Face Recognition'


Modules used in program: 
* `import os`
* `import face_recognition as fr`

## python Face Recognition

Python PIL example: Face Recognition

```python
import face_recognition as fr
from PIL import Image
import os

os.chdir("C:/Users/Asus/Pictures/")

# Load the jpg file into a numpy array

image = fr.load_image_file("new1.jpg")

# Find all the faces in the image 

face_locations = fr.face_locations(image)

print("I found {} face(s) in this photograph.".format(len(face_locations)))

i = 0
for face_location in face_locations:

    # Print the location of each face in this image
    top, right, bottom, left = face_location
    print("A face is located at pixel location Top: {}, Left: {}, Bottom: {}, Right: {}".format(top, left, bottom, right))

    # You can access the actual face itself like this:
    face_image = image[top:bottom, left:right]
    pil_image = Image.fromarray(face_image)
    # pil_image.show()
    pil_image.save("test1{}.jpg".format(i))
    i= i + 1
 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
