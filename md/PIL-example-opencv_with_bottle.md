---
title: PIL example opencv with bottle (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'opencv with bottle'

Functions in program: 
* `def machine_page_pst():`
* `def machine_learn():`
* `def machine_page_pst():`
* `def machine_page_pst():`
* `def machine_page_tmp():`
* `def home_page_tmp():`

Modules used in program: 
* `import shutil`
* `import numpy as np`
* `import cv2`
* `import os`

## python opencv with bottle

Python PIL example: opencv with bottle

```python
from bottle import default_app, route, template, request
import os
import cv2
import numpy as np
from PIL import Image
import shutil

@route('/')
def home_page_tmp():
    return "This is the homepage"

@route('/machine')
def machine_page_tmp():
    return template("/home/mysite/machine")

@route('/machine', method='POST')
def machine_page_pst():
    mlimg = request.files.get("mlimg")
    fname = mlimg.filename
    filepath = "/home/mysite/assets/uploaded/"
    mlimg.save(filepath + fname, overwrite = True)
    full_path = filepath + fname
    read_img = cv2.imread(full_path, 1)
    cascPath = "/home/mysite/haarcascade_frontalface_default.xml"
    faceCascade = cv2.CascadeClassifier(cascPath)
    faces = faceCascade.detectMultiScale(
        read_img,
        scaleFactor = 1.1,
        minNeighbors = 5,
        minSize=(50,50))
    how_many_faces = len(faces)
    for (x, y, w, h) in faces:
        cv2.rectangle(read_img, (x, y), (x+w, y+h), (255, 255, 255), 2)
    cv2.imwrite("/home/mysite/assets/uploaded/read.png", read_img)
    source = "/static/uploaded/read.png"
    return template("/home/mysite/machineout", source = source, how_many_faces = how_many_faces)

@route('/machine_split', method='POST')
def machine_page_pst():
    mlimg = request.files.get("mlimg")
    fname = mlimg.filename
    filepath = "/home/mysite/assets/uploaded/"
    mlimg.save(filepath + fname, overwrite = True)
    full_path = filepath + fname
    cascPath = "/home/mysite/haarcascade_frontalface_default.xml"
    faceCascade = cv2.CascadeClassifier(cascPath)
    faceSamples = []
    pilImage = Image.open(full_path).convert('L')
    imageNp = np.array(pilImage,'uint8')
    faces = faceCascade.detectMultiScale(imageNp,
     scaleFactor = 1.1,
     minNeighbors = 5,
     minSize=(50,50)
    )
    for (x,y,w,h) in faces:
        faceSamples.append(imageNp[y:y+h,x:x+w])
    for x in range(len(faceSamples)):
        cv2.imwrite("/home/mysite/assets/uploaded/{}.png".format(x), faceSamples[x])
    our_dir_list = []
    for x in range(len(faceSamples)):
        source = "/static/uploaded/{}.png".format(x)
        our_dir_list.append(source)
    from_path = os.listdir("/home/mysite/assets/uploaded/")
    for x in from_path:
        if str(x) == str(mlimg.filename):
            os.remove("/home/mysite/assets/uploaded/" + str(x))
    return template("/home/mysite/machineout_split", source = our_dir_list)

@route('/machine_split2', method='POST')
def machine_learn():
    learn_name = request.forms.get("our_name")
    path = "/home/mysite/assets/faces/"
    imagePaths=[os.path.join(path,f) for f in os.listdir(path)]
    existing_face_names = []
    for imagePath in imagePaths:
       face_name = str(imagePath.split("/")[-1].split(".")[0])
       existing_face_names.append(face_name.lower())
    if learn_name in existing_face_names:
        # We decided to trim the application to only allow for a unique name for a specific face
        # This area can be expanded to allow the same name to be used for different faces
        return "Name must be unique"
    from_path = os.listdir("/home/mysite/assets/uploaded/")
    Ids=[]
    for imagePath in imagePaths:
        Id=os.path.split(imagePath)[-1].split(".")[1]
        Ids.append(int(Id))
    magic_num = max(Ids) + 1
    counter = 1
    for file in from_path:
        save_name = learn_name + "." +  str(magic_num) + "." + str(counter) + ".png"
        os.rename("/home/mysite/assets/uploaded/" + str(file), "/home/mysite/assets/faces/" + str(save_name))
        counter += 1
    return template("/home/mysite/machine_success")

@route('/machine_recognize', method='POST')
def machine_page_pst():
    recognizer = cv2.face.LBPHFaceRecognizer_create()
    detector = cv2.CascadeClassifier("/home/mysite/haarcascade_frontalface_default.xml");
    path = "/home/mysite/assets/faces/"
    imagePaths=[os.path.join(path,f) for f in os.listdir(path)]
    faceSamples= []
    Ids= []
    Idnames = []
    for imagePath in imagePaths:
        pilImage=Image.open(imagePath).convert('L')
        imageNp=np.array(pilImage,'uint8')
        Id=os.path.split(imagePath)[-1].split(".")[1]
        Id_name=os.path.split(imagePath)[-1].split(".")[0]
        faces=detector.detectMultiScale(imageNp)
        for (x,y,w,h) in faces:
            faceSamples.append(imageNp[y:y+h,x:x+w])
            Ids.append(int(Id))
            Idnames.append(Id_name)
    recognizer.train(faceSamples, np.array(Ids))
    
    # Save image again
    mlimg = request.files.get("mlimg")
    fname = mlimg.filename
    filepath = "/home/mysite/assets/temp/"
    mlimg.save(filepath + fname, overwrite = True)
    full_path = filepath + fname
    read_img = cv2.imread(full_path, 0)
    cascPath = "/home/mysite/haarcascade_frontalface_default.xml"
    faceCascade = cv2.CascadeClassifier(cascPath)
    faces = faceCascade.detectMultiScale(
        read_img,
        scaleFactor = 1.1,
        minNeighbors = 5,
        minSize=(50,50)
    )
    for (x, y, w, h) in faces:
        cv2.rectangle(read_img, (x, y), (x+w, y+h), (255, 255, 255), 2)
    Id, conf = recognizer.predict(read_img[y:y+h,x:x+w])
    id_we_need = Ids.index(Id)
    name_we_need = str(Idnames[id_we_need])
    if id_we_need == None:
        name_we_need = "Unknown"
    cv2.imwrite("/home/mysite/assets/temp/read.png", read_img)
    source = "/static/temp/read.png"
    return template("/home/mysite/machine_recognize", homie = name_we_need, source = source, )

application = default_app()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
