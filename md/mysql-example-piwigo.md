---
title: mysql example piwigo (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'piwigo'

Functions in program: 
* `def getPath(id=0):`

Modules used in program: 
* `import os.path, shutil`
* `import mysql.connector`

## python piwigo

Python mysql example: piwigo

```python
import mysql.connector
import os.path, shutil


SRC_PATH="/tmp/piwigo/upload/folder"
OUT_PATH="/tmp/piwigo_images_extracted/"

USER="dbuser"
PWD = "dbpassword"
DB  = "db"
HOST= "localhost"

dbcon = mysql.connector.connect(user=USER, password=PWD, host=HOST, database=DB)

filecursor = dbcon.cursor(buffered=True)
filecursor.execute("select id, file, path from images;")


def getPath(id=0):
    cur2 = dbcon.cursor(buffered=True)
    path_ = list()
    while(True):
        cur2.execute("SELECT name, id_uppercat from categories where categories.id=%d;" % (id))

        p, parentID = cur2.fetchone()
        path_.insert(0, p.replace("/", "_"))
        id = parentID
        if id is None:
            break
    return os.path.join(OUT_PATH, *path_)


for (id, file, path) in filecursor:
    catID = dbcon.cursor()
    catID.execute("SELECT category_id from image_category where image_id=%d;"%(id))
    catID = catID.fetchone()[0]

    src_path = os.path.join(SRC_PATH, path)
    out_path = getPath(catID)
    file_out = os.path.join(out_path, file.decode('utf-8'))

    print(f"{src_path} => {out_path}/{file.decode('utf-8')}: ", end="")
    if not os.path.exists(out_path):
        os.makedirs(out_path)

    if os.path.exists(src_path):
        if not os.path.exists(file_out):
            print("Copying...", end="")
            try:
                shutil.copy2(src_path, file_out)
                print("OK.")
            except Exception as e:
                print("FAIL: %s"%e)
        else:
            print("Do Nothing, Target already exists")
    else:
        print("Error: Source file does not exists")

dbcon.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
