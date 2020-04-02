---
title: PIL example FindFace (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'FindFace'


Modules used in program: 
* `import face_recognition`
* `import PIL.ImageDraw`
* `import PIL.Image`

## python FindFace

Python PIL example: FindFace

```python
import PIL.Image
import PIL.ImageDraw
import face_recognition

image = face_recognition.load_image_file("IMG_20190720_160311.jpg")

face_locations = face_recognition.face_locations(image)

number_of_faces = len(face_locations)

pil_image = PIL.Image.fromarray(image)

for face_location in face_locations:
    top, right, bottom, left = face_location
    draw = PIL.ImageDraw.Draw(pil_image)
    draw.rectangle([left,top, right, bottom],outline="red",width=5)

pil_image.show()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
