---
title: PIL example down (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'down'

Functions in program: 
* `def pre_image_check():`
* `def calculateParallel(arr, threads=2):`
* `def squareNumber(item):`
* `def read_csv_data(csv_path):`
* `def resize_image(image_name):`
* `def blank_check(name, pil_data):`
* `def chunks(l, n):`

Modules used in program: 
* `import time`
* `import numpy as np`
* `import argparse`
* `import os`
* `import shutil`
* `import requests`
* `import csv`

## python down

Python PIL example: down

```python
import csv
import requests
import shutil
import os
import argparse
import numpy as np
import time
from tqdm import tqdm
from PIL import Image, ImageOps
from concurrent.futures import ThreadPoolExecutor

blank_data = Image.open('blank.jpg')
blank2_data = Image.open('blank2.jpg')

blank_arr = np.array(blank_data)
blank2_arr = np.array(blank2_data)

original_path = '/root/original/'
resize_path = '/root/resize/'
csv_path = 'main.csv'
pbar = None


def chunks(l, n):
    """Yield successive n-sized chunks from l."""
    for i in range(0, len(l), n):
        yield l[i:i + n]


def blank_check(name, pil_data):
    pil_data = pil_data.convert('L')

    size_data = None 

    if pil_data.size == blank_data.size:
        size_data = blank_arr
    elif pil_data.size == blank2_data.size:
        size_data = blank2_arr
    else:
        return False
    
    image_differ = np.sum(np.array(pil_data) - size_data)
    
    # same case --> True
    if image_differ == 0:
        os.remove('{}{}'.format(original_path, name))
        return True
    else:
        return False


def resize_image(image_name):
    img = Image.open(original_path + image_name)

    if blank_check(image_name, img):
        return 

    fit_img = ImageOps.fit(img, (256, 256), Image.ANTIALIAS)
    fit_img.save(original_path + image_name)

    fit_img = ImageOps.fit(img, (28, 28), Image.ANTIALIAS)
    fit_img.save(resize_path + image_name)


def read_csv_data(csv_path):
    data_arr = []

    with open(csv_path, 'r') as theFile:
        reader = csv.DictReader(theFile)

        for line in reader:
            data_arr.append(line)

    return data_arr


def squareNumber(item):
    r = requests.get(item['original_url'], stream=True)
    if r.status_code == 200:
        with open('{}{}.jpg'.format(original_path, item['image_id']), 'wb') as f:
            r.raw.decode_content = True
            shutil.copyfileobj(r.raw, f)

        resize_image('{}.jpg'.format(item['image_id']))

    pbar.update(1)


# function to be mapped over
def calculateParallel(arr, threads=2):
    with ThreadPoolExecutor(max_workers=threads) as executor:
        executor.map(squareNumber, arr)


def pre_image_check():
    prev_down_files = os.listdir(resize_path)
    original_files = os.listdir(original_path)

    for name in os.listdir(original_path):
        if not name in prev_down_files:
            print(name + 'delete')
            os.remove('{}{}'.format(original_path, name))
        print(name)

if __name__ == "__main__":
    pre_image_check()

    temp_arr = read_csv_data(csv_path)
    unique_arr = []

    original_files = os.listdir(original_path)

    for item in temp_arr:
        if not '{}.jpg'.format(item['image_id']) in original_files:
            unique_arr.append(item)

    print(len(unique_arr))
    for tt in list(chunks(unique_arr, 1000)):
        pbar = tqdm(total=len(tt))
        calculateParallel(tt, 200)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
