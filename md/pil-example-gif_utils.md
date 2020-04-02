---
title: pil example gif utils (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'gif utils'

Functions in program: 
* `def resize(path, base_width=300):`
* `def rename(duration=0.3):`
* `def create_gif3(duration):`
* `def create_gif2(path, duration, skip=0):`
* `def create_gif(filenames, duration):`

Modules used in program: 
* `import PIL`
* `import pathlib`
* `import imageio`
* `import datetime`
* `import glob`
* `import sys`

## python gif utils

Python pil example: gif utils

```python
#gif.py 
import sys
import glob
import datetime
import imageio
import pathlib
#from progress.bar import Bar
import PIL
from PIL import Image


VALID_EXTENSIONS = ('png', 'jpg')
#bar = Bar('Processing', max=100)


def create_gif(filenames, duration):
    images = []
    for filename in filenames:
        images.append(imageio.imread(filename))
    output_file = 'Gif-%s.gif' % datetime.datetime.now().strftime('%Y-%M-%d-%H-%M-%S')
    imageio.mimsave(output_file, images, duration=duration)

def create_gif2(path, duration, skip=0):
    images = []
    file_names = []
    count = 0
    for f in glob.glob('{}/*.jpg'.format(path)):
        count += 1
        file_names.append(f)

#    bar = Bar('Loading...', max=int(count/5))

    for f in file_names[::12]:
        images.append(imageio.imread(f))
#        bar.next()
#    bar.finish()

    print('Saving...')
    output_file = 'Gif-{}.gif'.format(datetime.datetime.now().strftime('%Y-%M-%d-%H-%M-%S'))
    imageio.mimsave(output_file, images, duration=duration)

def create_gif3(duration):

    images = []
    for i in range(20):
        file_name = '{}.jpg'
        images.append(imageio.imread(f))

    output_file = 'Gif-{}.gif'.format(datetime.datetime.now().strftime('%Y-%M-%d-%H-%M-%S'))
    imageio.mimsave(output_file, images, duration=duration)

def rename(duration=0.3):

    file_names = []
    images = []
    for f in glob.glob('{}/*.jpg'.format('imgs/')):
        print(f)
        file_names.append(int(f[8:13]))

    file_names.sort()
    for f in file_names[:-10]:
        file_name = 'imgs/G00{}_1517651271610_high.jpg'.format(f)
        images.append(imageio.imread(file_name))
    
    for f in file_names[-10:]:
        file_name = 'imgs/G00{}_1517651496528_high.jpg'.format(f)
        images.append(imageio.imread(file_name))

    output_file = 'Gif-{}.gif'.format(datetime.datetime.now().strftime('%Y-%M-%d-%H-%M-%S'))
    imageio.mimsave(output_file, images, duration=duration)





def resize(path, base_width=300):
    for f in glob.glob('{}/*.jpg'.format(path)):
        img = Image.open(f)
        wpercent = (base_width / float(img.size[0]))
        hsize = int((float(img.size[1]) * float(wpercent)))
        img = img.resize((base_width, hsize), PIL.Image.ANTIALIAS)
        img.save(f)







if __name__ == "__main__":
#    create_gif2(path='/home/dries/Videos/GDG/Images/moisture_sensor', duration=0.1, skip=0)
#    resize(path='imgs/',base_width=600)
    rename()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
