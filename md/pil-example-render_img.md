---
title: pil example render img (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'render img'

Functions in program: 
* `def add_prob_bar(img, labels, probs):`
* `def bbox_on_img(img, bbox, text, thickness=3,`
* `def _matplotfig2data(fig):`
* `def _rounding(ratio, max_val):`

Modules used in program: 
* `import numpy as np`
* `import matplotlib.pyplot as plt`
* `import cv2`

## python render img

Python pil example: render img

```python
import cv2
from PIL import Image
import matplotlib.pyplot as plt
import numpy as np


def _rounding(ratio, max_val):
    """ Return a rounded valud from 0 to max_val - 1 """
    val = int(ratio * max_val)
    return min(max_val - 1, val)


def _matplotfig2data(fig):
    """ matplotlib fig to data. Return a cv2 image """
    # draw the renderer
    fig.canvas.draw()

    # Get the RGBA buffer from the figure
    w, h = fig.canvas.get_width_height()
    buf = np.frombuffer(fig.canvas.tostring_argb(), dtype=np.uint8)
    buf.shape = (w, h, 4)
    buf = np.roll(buf, 3, axis=2)
    img_pil = Image.frombytes("RGBA", (w, h), buf.tostring())
    np_img = np.array(img_pil.convert('RGB'))
    return np_img[:, :, ::-1]


def bbox_on_img(img, bbox, text, thickness=3,
                text_size=16, color=(0, 0, 0)):
    """
    :param img: A Pillow Image
    :param bbox: A tuple of left upper corner x, y and right down corner x, y
                 from 0
    :param text: A str
    :param color: Bounding box/text color
    :return A rendered img
    """
    h, w = img.shape[:2]

    # Draw bbox
    ul_x = _rounding(bbox[0], w)
    ul_y = _rounding(bbox[1], h)
    br_x = _rounding(bbox[2], w)
    br_y = _rounding(bbox[3], h)
    img = cv2.rectangle(img, (ul_x, ul_y), (br_x, br_y), color,
                        thickness=thickness)

    img = cv2.putText(img, text,
                      (ul_x, ul_y-5),
                      fontFace=cv2.FONT_HERSHEY_SIMPLEX,
                      fontScale=0.5,
                      color=(0, 0, 0),
                      thickness=thickness)
    return img


def add_prob_bar(img, labels, probs):
    """ add probablistic bar """
    height, width = img.shape[:2]

    # Generate histogram
    fig, axs = plt.subplots()
    axs.bar(labels, probs)
    axs.set_ylim([0, 1])
    axs.set_ylabel('probablity')
    data_img = _matplotfig2data(fig)
    data_img = cv2.resize(data_img, (int(width / 1.5), height))
    full_img = np.concatenate([img, data_img], axis=1)
    return full_img


img = cv2.imread('sample.jpg')
img = add_prob_bar(img, ['dog', 'cat', 'butterfly'], [0.4, 0.5, 0.1])
img_pil = Image.fromarray(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
img_pil.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
