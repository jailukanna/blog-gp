---
title: pil example opencv test (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'opencv test'

Functions in program: 
* `def url_to_image(url):`

Modules used in program: 
* `import sys`
* `import cv2`
* `import urllib`
* `import numpy as np`

## python opencv test

Python pil example: opencv test

```python
# import the necessary packages
import numpy as np
import urllib
import cv2
from PIL import Image
import sys

class Tint:
    '''
    Tint
    Select the rgb value of a color tone and multiply it with he frame to create 
    a tinted effect into your image.
    '''
    def __init__(self):
        pass
    #create mask and render frame
    def render(self,frame,r,g,bl,s):
        temp=np.full((l,b,n),[bl,g,r],dtype=np.int)
        
        return np.multiply(frame,temp)

 
# METHOD #1: OpenCV, NumPy, and urllib
def url_to_image(url):
    # download the image, convert it to a NumPy array, and then read
    # it into OpenCV format
    resp = urllib.urlopen(url)
    image = np.asarray(bytearray(resp.read()), dtype="uint8")
    image = cv2.imdecode(image, cv2.IMREAD_COLOR) #equivalent to cv2.imread()
    o_b,o_g,o_r = cv2.split(image)

    rgb_img = cv2.merge([o_r,o_g,o_b]) #convert to rgb_image    
    pil_img = Image.fromarray(rgb_img, 'RGB')
    height = pil_img.height
    width = pil_img.width
    
    r = 255
    g = 255
    bl = 255
    
    #temp=np.full((height,width),[bl,g,r],dtype=np.int)
    rgb_image = Image.new('RGB', (height,width), (r, g, bl))
    #open_cv_image = np.array(rgb_image) 
    open_cv_image = cv2.cvtColor(np.array(rgb_image), cv2.COLOR_RGB2BGR)
    #image = cv2.imdecode(open_cv_image, cv2.IMREAD_COLOR) #equivalent to cv2.imread()
    #o_b,o_g,o_r = cv2.split(open_cv_image)
    
    #open_cv_image = np.full((height,width, 3),[bl,g,r],dtype=np.int)
    new_image = np.multiply(image,open_cv_image)
    
    #filtered = cv2.merge([bl, g, r])
    
    ##h6=Tint()
    #ret, frame = image.read()
    #cv2.imshow("Tint",h6.render(frame,r,g,bl,s))
    #cv2.imwrite("Mozaic.jpg",h6.render(frame))
    #cv2.imwrite("WaterColor.jpg",h6.render(frame))
    #cap.release()
    
    o_b,o_g,o_r = cv2.split(new_image)
    rgb_img = cv2.merge([o_r,o_g,o_b]) #convert to rgb_image 
    
    img = Image.fromarray(rgb_img, 'RGB')
    img.save('my.jpg', 'JPEG')


if __name__ == "__main__":
    url_to_image('https://s3.amazonaws.com/fetesting2/__fp2.jpg')
    sys.exit(0)
    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
