---
title: pil example the-walking-dead (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'the-walking-dead'


## python the-walking-dead

Python pil example: the-walking-dead

```python
# Author: Felipe Fronchetti
# Folow me on GitHub: https://github.com/fronchetti
# All image rights reserved for AMC Network Entertainment.
# Please, reproduce this algorithm with common sense!

try:
    import os
    from urllib2 import urlopen
    from urllib import urlretrieve
    from bs4 import BeautifulSoup
    from PIL import Image, ImageDraw, ImageFont
    import face_recognition
except ImportError:
    raise ImportError()

dataset = "./Dataset/"
output = "./Output/"

class Dataset():
    def __init__(self):
        self.urls = ["http://www.amc.com/extras/season-2-zombie-photos", "http://www.amc.com/shows/the-walking-dead/extras/the-walking-dead-season-3-zombie-photos"]

    def create(self):
        if not os.path.isdir(dataset):
            os.makedirs(dataset)

        for url in self.urls:
            html = urlopen(url).read()
            soup = BeautifulSoup(html, "html.parser")
            images = soup.findAll("img", {"class":"rsImg"})

            for image in images:
                image_url = image.get("src")
                image_file = image_url.split("/")[-1]
                if not os.path.isfile(dataset + image_file):
                    urlretrieve(image_url, dataset + image_file)

class FaceRecognition():
    def start(self):
        if not os.path.isdir(output):
            os.makedirs(output)

        if os.path.isdir(dataset):
            for image_file in os.listdir(dataset):
                image = face_recognition.load_image_file(dataset + image_file)
                face_locations = face_recognition.face_locations(image, number_of_times_to_upsample=0, model="cnn")

                pil_image = Image.fromarray(image)
                draw = ImageDraw.Draw(pil_image)
                font = ImageFont.truetype("/usr/share/fonts/truetype/freefont/FreeMonoBold.ttf", 15)
                width, height = pil_image.size
                draw.rectangle([0, height * 0.95, width, height], fill="#000000")
                draw.text((width * 0.05, height * 0.965), "Zombies detected: " + str(len(face_locations)) + " / All image rights reserved for AMC.")

                for face_location in face_locations:
                    top, right, bottom, left = face_location
                    draw.line(((left, bottom),(right, bottom)), width = 2, fill="#ff0000")
                    draw.line(((left, top), (right, top)), width = 2, fill="#ff0000")
                    draw.line(((left, bottom), (left, top)), width = 2, fill="#ff0000")
                    draw.line(((right, bottom), (right, top)), width = 2, fill="#ff0000")
                    draw.rectangle([left - 1, bottom, right, bottom + 20], fill="#ff0000")
                    draw.text((left + 1, bottom), "Zombie", font=font)

                pil_image.save(output + image_file.replace(".jpg","_final.png"), format="png")

D = Dataset()
D.create()
F = FaceRecognition()
F.start()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
