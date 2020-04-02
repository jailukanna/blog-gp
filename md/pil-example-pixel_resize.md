---
title: pil example pixel resize (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'pixel resize'


Modules used in program: 
* `import PIL, os, sys`

## python pixel resize

Python pil example: pixel resize

```python
import PIL, os, sys
from PIL import Image
'''options = 
1. 100 pixel          2. 200 pixel
3. 300 pixel          4. 400 pixel
5. 500 pixel          6. 600 pixel

os.system('cls')
print(options)
option = raw_input('Select: ')
if option == "0":
	print("Error! 0 is not valid input, Select from 1 ,2 ,3 ,4...")
	sys.exit(1)
elif option == "1":
	baseheight = 100
elif option == "2":
	baseheight = 200
elif option == "3":
	baseheight = 300
elif option == "4":
	baseheight = 400
elif option == "5":
	baseheight = 500
elif option == "6":
	baseheight = 600
else:
	print("Error! No input")
	sys.exit(1)
imagename = raw_input('\nImage name: ')
img = Image.open(imagename)
hpercent = (baseheight / float(img.size[1]))
wsize = int((float(img.size[0]) * float(hpercent)))
img = img.resize((wsize, baseheight), PIL.Image.ANTIALIAS)
newname = '%s_px_%s'%(baseheight,imagename)
print("\nFile saved as : "+newname)
img.save(newname)
'''
options = '''

Input Pixels i.e. 100, 500....
Input Image name i.e 1337.jpg

'''
os.system('cls')
print(options)
try:
	baseheight = int(raw_input('Pixels : '))
except KeyboardInterrupt:
	print("\n\nExit using CTRL+C !")
	sys.exit(1)
imagename = raw_input('\nImage name: ')
img = Image.open(imagename)
hpercent = (baseheight / float(img.size[1]))
wsize = int((float(img.size[0]) * float(hpercent)))
img = img.resize((wsize, baseheight), PIL.Image.ANTIALIAS)
newname = '%s_px_%s'%(baseheight,imagename)
print("\nFile saved as : "+newname)
img.save(newname)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
