---
title: pil example Image%2520Recongised%2520code (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'Image%2520Recongised%2520code'


Modules used in program: 
* `import pytesseract`

## python Image%2520Recongised%2520code

Python pil example: Image%2520Recongised%2520code

```python
#
#
from PIL import Image
import pytesseract
image = Image.open('2.jpg')
orange = pytesseract.image_to_string(image)
print(orange)


#Record the preparation process is very troublesome.
#First library: pytesseract,image，tesseract，PIL
#Windows installation PIL, direct exe to install more convenient (https://files.cnblogs.com/files/Oran9e/PILwin64.zip) (https://files.cnblogs.com/files/Oran9e/PILwin32.zip)
#Install image:pip install image
#Install pytesseract: pip install pytesseract
#Install tesseract: pip install tesseract (install tesseracr, here is a pit, you need to install to the C drive C: \ Program Files (x86) \ Tesseract-OCR, which is the default path, otherwise you can not call tesseract.exe when running python code)
#Modify the tesseract.py code: \python\Lib\site-packages\pytesseract\tesseract.py
#Tesseract_cmd changed to the tesseract.exe path and called.
tesseract_cmd = 'C:/Program Files (x86)/Tesseract-OCR/tesseract.exe'
#After preparing the above work, basically a simple verification code can be identified.

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
