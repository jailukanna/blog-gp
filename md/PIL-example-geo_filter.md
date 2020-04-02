---
title: PIL example geo filter (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'geo filter'

Functions in program: 
* `def configureLogging(export_path):`
* `def searchAndDestroy(root_dir, manual_check_dir):`
* `def getPhotoFilePaths(root_dir):`
* `def hasGpsData(file_path):`
* `def getExif(file_path):`

Modules used in program: 
* `import shutil`
* `import os`
* `import PIL.ExifTags`
* `import logging`
* `import PIL`
* `import PIL.Image as img`

## python geo filter

Python PIL example: geo filter

```python
import PIL.Image as img
import PIL
import logging
import PIL.ExifTags
import os
import shutil

GPS_TAGS = ['gpslatitude','gpsinfo','gps','gps latitude','gps position','gps altitude', 'latitude', 'position']

def getExif(file_path):
    file = img.open(file_path)
    data = file._getexif()

    if data is None:
        data = {}
    else:
        data = {PIL.ExifTags.TAGS[k]: v for k, v in data.items()if k in PIL.ExifTags.TAGS}

    return data


def hasGpsData(file_path):
    global GPS_TAGS

    data = getExif(file_path)
    for x in data.keys():
        if x.lower() in GPS_TAGS:
            return True

    return False


def getPhotoFilePaths(root_dir):
    for dirpath, dirnames, filenames in os.walk(root_dir):
        for x in filenames:
            if x.split('.')[-1].lower() in ['jpg', 'jpeg']:
                file_path = os.path.join(dirpath, x)
                yield x, file_path


# Takes a directory and scans all jpg files in that directory and subdirectories
# deletes any photo which don't have geo location data
def searchAndDestroy(root_dir, manual_check_dir):
    for file_name, file_path in getPhotoFilePaths(root_dir):

        try:
            if not hasGpsData(file_path):
                logging.info('Removing file: {}'.format(file_path))
                os.remove(file_path)
        except OSError as e:
            logging.error('Could not process file: {} due to OS Error'.format(file_path))
            shutil.move(file_path, os.path.join(manual_check_dir, file_name))


def configureLogging(export_path):
    # Logs will be sent to console and a file
    log_file_path = os.path.join(export_path, 'app.log')
    logging.basicConfig(filename=log_file_path, level=logging.DEBUG, format='%(asctime)s %(levelname)s %(message)s')
    logging.getLogger().addHandler(logging.StreamHandler())


configureLogging('./')

logging.info('----------------START---------------')

manual_check_folder = './manual_check/'
if not os.path.isdir(manual_check_folder):
    os.mkdir(manual_check_folder)

searchAndDestroy(r'G:\test', manual_check_folder)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
