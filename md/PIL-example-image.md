---
title: PIL example image (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'image'

Functions in program: 
* `def analyse_image(image_fname):`
* `def pil_2_cv2(pimg):`
* `def load_templates():`
* `def cv2_2_pil(cimg):`
* `def captch_ex(orig_image, iterations, dx, dy, thresh_min):`

Modules used in program: 
* `import numpy as np`
* `import cv2`

## python image

Python PIL example: image

```python
import cv2
import numpy as np
def captch_ex(orig_image, iterations, dx, dy, thresh_min):
    img = orig_image.copy()
    #  remove red teal and green colors
    img[np.where((img == [255, 255, 127]).all(axis=2))] = np.array([255, 255, 255])
    img[np.where((img == [0, 0, 255]).all(axis=2))] = np.array([0, 0, 0])
    img[np.where((img == [0, 127, 0]).all(axis=2))] = np.array([0, 0, 0])
    img2gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    ret, mask = cv2.threshold(img2gray, thresh_min, 255, cv2.THRESH_BINARY)
    image_final = cv2.bitwise_and(img2gray, img2gray, mask=mask)
    ret, new_img = cv2.threshold(image_final, thresh_min, 255, cv2.THRESH_OTSU)
    return new_img

def cv2_2_pil(cimg):
    return Image.fromarray(cv2.cvtColor(cimg, cv2.COLOR_BGR2RGB))

def load_templates():
    import glob
    return [(cv2.imread(t, 0), t) for t in glob.glob('scats_template/*.png')]

def pil_2_cv2(pimg):
    return cv2.cvtColor(np.array(pimg), cv2.COLOR_RGB2BGR)
    
def analyse_image(image_fname):
    templates = load_templates()
   
    img = Image.open(image_fname)
    cimg = captch_ex(pil_2_cv2(img), 7, 1, 1, 127)

    cimg = (255 - cimg)
    # 'cv2.TM_CCOEFF', 'cv2.TM_CCOEFF_NORMED', 'cv2.TM_CCORR',
    # 'cv2.TM_CCORR_NORMED', 'cv2.TM_SQDIFF',
    # http://docs.opencv.org/3.1.0/d4/dc6/tutorial_py_template_matching.html#gsc.tab=0
    for m in ['cv2.TM_CCOEFF_NORMED']:
        method = eval(m)
        color_img = cv2.cvtColor(cimg, cv2.COLOR_GRAY2RGB)
        for t in templates:
            template, name = t[0], t[1].split('/')[1][:-4]
            w, h = template.shape
            res = cv2.matchTemplate(cimg, template, method)
            threshold = 0.75
            loc = np.where(res >= threshold)
            for pt in zip(*loc[::-1]):
                cv2.rectangle(color_img, pt, (pt[0] + w, pt[1] + h), (0, 0, 255), 2)
                cv2.putText(color_img, name, pt, cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 127), 2, cv2.LINE_AA)

        img = cv2_2_pil(color_img)
        img.show(title=m)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
