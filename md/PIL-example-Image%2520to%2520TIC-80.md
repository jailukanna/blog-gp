---
title: PIL example Image%2520to%2520TIC-80 (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'Image%2520to%2520TIC-80'


Modules used in program: 
* `import os`
* `import PIL`

## python Image%2520to%2520TIC-80

Python PIL example: Image%2520to%2520TIC-80

```python
import PIL
from PIL import Image
from PIL import ImageGrab
import os
while(True):
	while(True):
		imageName=input("Enter filename: ")
		if(os.path.isfile(imageName)): #File exists?
			image=Image.open(imageName)
			break
		elif(imageName.strip().lower()=="clipboard"):
			image=ImageGrab.grabclipboard()
			break
		else:
			print("\""+imageName+"\" doesn't exist.")
	palette="140c1c44243430346d4e4a4e854c30346524d04648757161597dced27d2c8595a16daa2cd2aa996dc2cadad45edeeed6"
	colors=[[int(palette[j:j+2],16)for j in range(i,i+6,2)]for i in range(0,len(palette),6)] #Converted into a list.
	availableSizes=[8,16,32,64]
	while(True):
		chosenSize=input("Choose size: "+str(availableSizes)+"\n")
		if(any(chosenSize==str(e) for e in availableSizes)):
			chosenSize=int(chosenSize)
			break
		elif(image.size[0]==image.size[1]and image.size[0]in availableSizes):
			print("I will use the original size.")
			chosenSize=image.size[0]
			break
		else:
			print("No such size as "+chosenSize+".")
	image=image.resize((chosenSize,chosenSize),PIL.Image.ANTIALIAS)
	pix=image.load()
	def colorDifference(a,b):
		return(abs(a[0]-b[0])+abs(a[1]-b[1])+abs(a[2]-b[2]))/3.0
	string=""
	for yy in range(0,image.size[1]):
		for xx in range(0,image.size[0]):
			diff_list=[]
			for i in colors:
				diff_list.append(colorDifference(pix[xx,yy],i))
			string+=format(diff_list.index(min(diff_list)),"x") #format converts decimal to hexadecimal.
	print(string)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
