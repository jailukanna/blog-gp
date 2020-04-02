---
title: PIL example face recognition (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'face recognition'

Functions in program: 
* `def draw(image, locations, faces, file_name):`
* `def recognize(known_encodings, known_names, encodings):`
* `def process_image(file_name):`
* `def download(filename, url):`
* `def restore():`
* `def recognize():`
* `def process(url, name):`
* `def upload():`

Modules used in program: 
* `import face_recognition`
* `import requests`
* `import numpy`
* `import os`

## python face recognition

Python PIL example: face recognition

```python
#!flask/bin/python
from flask import Flask, request, send_file
from PIL import Image, ImageDraw

import os
import numpy
import requests
import face_recognition

requests.packages.urllib3.disable_warnings()

app = Flask(__name__)


@app.route('/upload', methods=['GET'])
def upload():
    raw_url = request.args['url']
    raw_name = request.args['name']

    photo, name = process(raw_url, raw_name)

    if photo is not None:
        numpy.savetxt('./memory/%s.csv' % name, photo, delimiter=',')
        print('%s\'s face uploaded' % name)
        return 'Face of %s is successfully recognized' % raw_name
    else:
        return 'Photo recognizing is failed'


def process(url, name):
    name = name.replace(' ', '_')
    file = download('./known_images/' + name + '.jpg', url)
    if file:
        image = face_recognition.load_image_file(file)
        encodings = face_recognition.face_encodings(image, face_recognition.face_locations(image))
        return encodings[0] if len(encodings) > 0 else None, name
    else:
        return None


@app.route('/recognize', methods=['GET'])
def recognize():
    image, locations, encodings = process_image(
        download('picture.jpg', request.args['url'])
    )

    photos, names = restore()

    faces = list(recognize(photos, names, encodings))

    draw(image, locations, faces, 'recognized_picture.jpg')

    for face in faces:
        print('%s\'s face recognized' % face)
    print('')

    return send_file(
        'recognized_picture.jpg',
        mimetype='image/jpg'
    )


def restore():
    photos, names = [], []

    for name in os.listdir('./memory'):
        photos.append(numpy.loadtxt('./memory/%s.csv' % name, delimiter=','))
        names.append(name)

    return photos, names


def download(filename, url):
    response = requests.get(url, verify=False, stream=True)
    if response.status_code == 200:
        with open(filename, 'wb') as f:
            f.write(response.content)
        return filename
    else:
        return None


def process_image(file_name):
    image = face_recognition.load_image_file(file_name)
    locations = face_recognition.face_locations(image)
    encodings = face_recognition.face_encodings(image, locations)
    return image, locations, encodings


def recognize(known_encodings, known_names, encodings):
    for encoding in encodings:
        matches = face_recognition.compare_faces(known_encodings, encoding, tolerance=0.8)

        best_match_index = numpy.argmin(
            face_recognition.face_distance(known_encodings, encoding)
        )
        yield known_names[best_match_index] if matches[best_match_index] else "Unknown"


def draw(image, locations, faces, file_name):
    pil_image = Image.fromarray(image)
    drawing = ImageDraw.Draw(pil_image)

    for (top, right, bottom, left), name in zip(locations, faces):
        drawing.rectangle(((left, top), (right, bottom)), outline=(255, 0, 0), width=2)
        drawing.text((left + 10, bottom - 10), name)

    pil_image.save(file_name)


if __name__ == '__main__':
    app.run(debug=True, port=8080)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
