---
title: matplotlib example circular (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'circular'


Modules used in program: 
* `import matplotlib.cbook as cbook`
* `import matplotlib.patches as patches`
* `import matplotlib.pyplot as plt`

## python circular

Python matplotlib example: circular

```python
import matplotlib.pyplot as plt
import matplotlib.patches as patches
import matplotlib.cbook as cbook


#big trouble!!!!!! get_sample_data() the first arjyment is pathrelative the /home/ss/.local/lib/python3.6/site-packages/matplotlib/mpl-data/sample_data/   and some sample images in this path
with cbook.get_sample_data('grace_hopper.png') as image_file:
    image = plt.imread(image_file)


# image = plt.imread('circular.jpg')

fig, ax = plt.subplots()
im = ax.imshow(image)
patch = patches.Circle((260, 200), radius=200, transform=ax.transData)
im.set_clip_path(patch)

ax.axis('off')
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
