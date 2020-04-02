---
title: PIL example RBG histogram (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'RBG histogram'


Modules used in program: 
* `import math `
* `import matplotlib.pyplot as plt`

## python RBG histogram

Python PIL example: RBG histogram

```python
from PIL import Image
import matplotlib.pyplot as plt
import math 
# image on which histogram is to be plotted 
img = Image.open("nature.jpg") 
pixels = img.load()
# for storing key values 
# key is the pixel intensity 
# values are the number of pixels 
redMap = {} 	                    # for red
greenMap = {} 	                    # for green
blueMap = {}	                    # for blue
for i in range(img.size[0]):
    for j in range(img.size[1]):
    	r, g, b = pixels[i, j][0], pixels[i, j][1], pixels[i, j][2];
    	if redMap.get(r) == None:
    		redMap[r] = 1;
    	else:
    		redMap[r] = redMap.get(r)+1;
    	if greenMap.get(g) == None:
    		greenMap[g] = 1;
    	else:
    		greenMap[g] = greenMap.get(g)+1;
    	if blueMap.get(b) == None:
    		blueMap[b] = 1;
    	else:
    		blueMap[b] = blueMap.get(b)+1;
plt.bar(list(redMap.keys()), redMap.values(), color='r')
plt.show()
plt.bar(list(greenMap.keys()), greenMap.values(), color='g')
plt.show()
plt.bar(list(blueMap.keys()), blueMap.values(), color='b')
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
