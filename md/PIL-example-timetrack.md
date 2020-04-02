---
title: PIL example timetrack (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'timetrack'


Modules used in program: 
* `import subprocess`
* `import logging`
* `import os`
* `import sqlite3`
* `import datetime`
* `import time`
* `import face_recognition`

## python timetrack

Python PIL example: timetrack

```python
from PIL import Image
import face_recognition

import time
import datetime
import sqlite3
from sqlite3 import Error
import os
import logging
import subprocess

timestamp = datetime.datetime.now()
date = timestamp.strftime('%d-%m-%Y')

webcam_file = os.path.join(os.path.dirname(os.path.realpath(__file__)), "webcam.jpg")
#~ save_file = os.path.join(os.path.dirname(os.path.realpath(__file__)), "sf_")

log_file = os.path.join(os.path.dirname(os.path.realpath(__file__)), "timetrack.log")
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger()
handler = logging.FileHandler(log_file)
handler.setLevel(logging.DEBUG)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)
logger.addHandler(handler)

db_file = os.path.join(os.path.dirname(os.path.realpath(__file__)), "timetrack.db")
sql_create_table = """ CREATE TABLE IF NOT EXISTS entries (
                   id integer PRIMARY KEY,
                   date text NOT NULL,
                   timestamp text,
                   comment text); """
                   
try:
    conn = sqlite3.connect(db_file)
    try:
        c = conn.cursor()
        c.execute(sql_create_table)
    except Error as e:
        print(e)
except Error as e:
    logger.debug(e)
finally:
    conn.close()                   

subprocess.call("fswebcam -q -r 640x480 --jpeg 85 {0}".format(webcam_file), shell=True, stderr=subprocess.STDOUT)

# Load the jpg file into a numpy array
image = face_recognition.load_image_file(webcam_file)

# Find all the faces in the image using a pre-trained convolutional neural network.
# This method is more accurate than the default HOG model, but it's slower
# unless you have an nvidia GPU and dlib compiled with CUDA extensions. But if you do,
# this will use GPU acceleration and perform well.
# See also: find_faces_in_picture.py
face_locations = face_recognition.face_locations(image, number_of_times_to_upsample=0, model="cnn")

#print("I found {} face(s) in this photograph.".format(len(face_locations)))

for face_location in face_locations:

    #~ # Print the location of each face in this image
    #~ top, right, bottom, left = face_location
    #~ # print("A face is located at pixel location Top: {}, Left: {}, Bottom: {}, Right: {}".format(top, left, bottom, right))

    #~ # You can access the actual face itself like this:
    #~ face_image = image[top:bottom, left:right]
    #~ pil_image = Image.fromarray(face_image)
    #~ pil_image.save(str(save_file) + str(int(time.time())) + ".jpg")
    
    comment = "user present"
    
    logger.info('DB INSERT: user present')
    data = [date, timestamp, comment]
    sql_new_entry = """INSERT INTO entries(date, timestamp, comment)
                       VALUES (?,?,?); """
    conn = sqlite3.connect(db_file)
    try:
        c = conn.cursor()
        c.execute(sql_new_entry, data)
        conn.commit()
    except Error as e:
        logger.debug(e)
    finally:
        conn.close()
   
if len(face_locations) == 0:
    comment = "empty"
    
    logger.info('DB INSERT: place is empty')
    data = [date, timestamp, comment]
    sql_new_entry = """INSERT INTO entries(date, timestamp, comment)
                       VALUES (?,?,?); """
    conn = sqlite3.connect(db_file)
    try:
        c = conn.cursor()
        c.execute(sql_new_entry, data)
        conn.commit()
    except Error as e:
        logger.debug(e)
    finally:
        conn.close()    


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
