---
title: PIL example bal (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'bal'

Functions in program: 
* `def createImages(num, file_name, tags_file_name):`

Modules used in program: 
* `import numpy as np`
* `import random as rn`
* `import cv2`

## python bal

Python PIL example: bal

```python
import cv2
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont
import random as rn

 
# load the image and show it
numOfDigits=1
cols=64
rows=64*numOfDigits



import numpy as np

def createImages(num, file_name, tags_file_name):
	tags=open(tags_file_name,"w")
	for i in range (1,num): #how many files
		pattern = Image.new("RGB", (rows, cols), "black")
		size = width, height = pattern.size
		draw = ImageDraw.Draw(pattern)
		fontSize=rn.randint(29,32)  #range of font size
		font = ImageFont.truetype("/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf", fontSize)
		digit=rn.randint(0,9)
		positionX=rn.randint(22,23)  #range of position
		positionY=rn.randint(10,30) #range of position
		#this one is written in the file with spaces 
		num_string= str(digit )
		#this string goes into the csv file, we don't want whitespaces there
		num_string_csv=str(digit)
		draw.text((positionX,positionY), num_string, (255, 255, 255, 255),font=font)
		image=np.array(pattern)

		#cv2.imshow("original", image)
		rotationAngle=rn.randint(-9,9) #range of rotation
		M = cv2.getRotationMatrix2D((cols/2,rows/2),rotationAngle,1)
		white_image = np.ones(cols, np.uint8)
		image = cv2.warpAffine(image, M, (rows, cols),
		flags=cv2.INTER_CUBIC, borderMode=cv2.BORDER_REPLICATE)

		stretchHorizontal=rn.uniform(1,1.1)  #range of stretch
		stretchVertical=rn.uniform(1,1.1) # range of stretch

		image = cv2.resize(image,None,fx=stretchHorizontal, fy=stretchVertical, interpolation = cv2.INTER_CUBIC)
		image = image[0:cols,0:rows]
		cv2.imshow("modified",image)
		cv2.imwrite(file_name +str(digit)+"_"+str(i)+".png", image );
		tags.write(num_string_csv+","+file_name +str(digit)+"_"+str(i)+".png"+ "\n")
	tags.close

	
createImages(50001, "/home/peter/tensorflow_notes/lesson4_single_number_cnn/data/","/home/peter/tensorflow_notes/lesson4_single_number_cnn/data/tags.csv")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
