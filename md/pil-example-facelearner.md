---
title: pil example facelearner (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'facelearner'


Modules used in program: 
* `import numpy as np`
* `import pickle`
* `import os.path`
* `import csv`
* `import urllib.request`
* `import csv`
* `import face_recognition`

## python facelearner

Python pil example: facelearner

```python
import face_recognition
import csv
from PIL import Image, ImageDraw
import urllib.request
import csv
import os.path
import pickle
import numpy as np

# Create arrays of known face encodings and their names
known_face_encodings = [
]
known_face_names = [
]

if os.path.isfile('politician_faces.pkl'):
    with open('politician_faces.pkl', 'rb') as input:
        known_face_encodings = pickle.load(input)
        known_face_names = pickle.load(input)
else:
    with open('dia21_11_politicos.csv') as csvfile:
        spamreader = csv.reader(csvfile, delimiter=',', quotechar='|', )
        for row in list(spamreader)[1:]:
            print("Loading face of " + row[-1])
            # Download the file from `url` and save it locally under `file_name`:
            path = "fotos/{}.jpg".format(row[-1])
            urllib.request.urlretrieve(row[4], path)
            image = face_recognition.load_image_file(path)
            face_encoding = face_recognition.face_encodings(image, num_jitters=10)[0]
            known_face_encodings.append(face_encoding)
            known_face_names.append(row[-1])
    with open('politician_faces.pkl', 'wb') as output:
        pickle.dump(known_face_encodings, output, pickle.HIGHEST_PROTOCOL)
        pickle.dump(known_face_names, output, pickle.HIGHEST_PROTOCOL)


# This is an example of running face recognition on a single image
# and drawing a box around each person that was identified.

images = ["camara.jpg", "camara2.jpg", "camara3.jpg", "camara4.jpg"]
new_faces = 0

for i, image in enumerate(images):
    base_image = face_recognition.load_image_file(image)
    face_locations = face_recognition.face_locations(base_image)
    face_encodings = face_recognition.face_encodings(base_image, face_locations, num_jitters=3)
    pil_image = Image.fromarray(base_image)
    draw = ImageDraw.Draw(pil_image)
    # Loop through each face found in the unknown image
    for (top, right, bottom, left), face_encoding in zip(face_locations, face_encodings):
        # See if the face is a match for the known face(s)
        distances = face_recognition.face_distance(known_face_encodings, face_encoding)

        # If a match was found in known_face_encodings, just use the first one.
        if min(distances) < 0.48:
            nearst = np.argmin(distances)
            old_encoding = known_face_encodings[nearst]
            known_face_encodings[nearst] = (np.asarray(old_encoding) + np.asarray(face_encoding)) / 2.0
            name = known_face_names[nearst]
            print("Found " + name)
        else:
            known_face_encodings.append(face_encoding)
            known_face_names.append("Face " + str(i) + "#" + str(new_faces))
            new_faces += 1
            name = known_face_names[-1]
            print("Found new person")

        # Draw a box around the face using the Pillow module
        draw.rectangle(((left, top), (right, bottom)), outline=(0, 0, 255))

        # Draw a label with a name below the face
        text_width, text_height = draw.textsize(name)
        draw.rectangle(((left, bottom + text_height + 10), (right, bottom)), fill=(0, 0, 255), outline=(0, 0, 255))
        draw.text((left, bottom + text_height), name, fill=(255, 255, 255, 255))
    # Remove the drawing library from memory as per the Pillow docs
    del draw

    # Display the resulting image
    pil_image.show()

# You can also save a copy of the new image to disk if you want by uncommenting this line
# pil_image.save("image_with_boxes.jpg")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
