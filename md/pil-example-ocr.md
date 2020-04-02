---
title: pil example ocr (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'ocr'


Modules used in program: 
* `import os # operating system`
* `import cv2 # opencv-python`
* `import argparse # build command-line interfaces`
* `import pytesseract # python "wrapper" of Google's Tesseract binaries`

## python ocr

Python pil example: ocr

```python
# import the necessary packages
from PIL import Image # get image from disk in PIL format
import pytesseract # python "wrapper" of Google's Tesseract binaries
import argparse # build command-line interfaces
import cv2 # opencv-python
import os # operating system


# construct the argument parse and parse the arguments
ap = argparse.ArgumentParser()
ap.add_argument("-i",
                "--image",
                required=True,
                help="path to input image to be OCR'd")
ap.add_argument("-p",
                "--preprocess",
                type=str,
                default="thresh",
                help="type of preprocessing to be done")
args = vars(ap.parse_args())


# load the example image and convert it to grayscale
image = cv2.imread(args["image"])

# we need to keep in mind aspect ratio so the image does
# not look skewed or distorted -- therefore, we calculate
# the ratio of the new image to the old image
r = 1000.0 / image.shape[1]
dim = (1000, int(image.shape[0] * r))
 
# perform the actual resizing of the image and show it
image = cv2.resize(image, dim, interpolation = cv2.INTER_AREA)
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# check to see if we should apply thresholding to preprocess the image
if args["preprocess"] == "thresh":
    gray = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY | cv2.THRESH_OTSU)[1]
    
# make a check to see if median blurring should be done to remove noise
elif args["preprocess"] == "blur":
    gray = cv2.medianBlur(gray, 3)

# make a check to see if median blurring should be done to remove noise
elif args["preprocess"] == "gauss":
    gray = cv2.GaussianBlur(gray, (3,3), 255)
    
# write the grayscale image to disk as a temporary file so we can apply OCR to it
filename = "{}.png".format(os.getpid())
cv2.imwrite(filename, gray)


# load the image as a PIL/Pillow image, apply OCR, and then delete the temporary file
text = pytesseract.image_to_string(Image.open(filename))
os.remove(filename)
print(text)

# show the output images
cv2.imshow("Image", image)
cv2.imshow("Output",  gray)
cv2.waitKey(0)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
