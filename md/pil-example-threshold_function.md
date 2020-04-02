---
title: pil example threshold function (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'threshold function'

Functions in program: 
* `def threshold(imageArray):`

Modules used in program: 
* `import time`
* `import matplotlib.pyplot as plt`
* `import numpy as np`

## python threshold function

Python pil example: threshold function

```python
import numpy as np
import matplotlib.pyplot as plt
import time
from PIL import Image

def threshold(imageArray):
	balAr = []
	newAr = imageArray
	from statistics import mean
	for eachRow in imageArray:
		for eachPix in eachRow:
			avg = mean(eachPix[:3])
			balAr.append(avg)
	balance = mean(balAr)

	for eachRow in newAr:
		for eachPix in eachRow:
			if mean(eachPix[:3]) > balance :
				eachPix[0] = 255
				eachPix[1] = 255
				eachPix[2] = 255
				eachPix[3] = 255
			else:
				eachPix[0] = 0
				eachPix[1] = 0
				eachPix[2] = 0
				eachPix[3] = 255
	return newAr		

i1 = Image.open('images/dot.png')
i1_ar = np.array(i1)
i1_ar = threshold(i1_ar)

i2 = Image.open('images/numbers/0.1.png')
i2_ar = np.array(i2)
i2_ar = threshold(i2_ar)

i3 = Image.open('images/numbers/y0.5.png')
i3_ar = np.array(i3)
i3_ar = threshold(i3_ar)

i4 = Image.open('images/dotndot.png')
i4_ar = np.array(i4)
i4_ar = threshold(i4_ar)

fig = plt.figure()

ax1 = plt.subplot2grid((8,6),(0,0),rowspan = 4,colspan = 3)
ax2 = plt.subplot2grid((8,6),(4,0),rowspan = 4,colspan = 3)
ax3 = plt.subplot2grid((8,6),(0,3),rowspan = 4,colspan = 3)
ax4 = plt.subplot2grid((8,6),(4,3),rowspan = 4,colspan = 3)

ax1.imshow(i1_ar)
ax2.imshow(i2_ar)
ax3.imshow(i3_ar)
ax4.imshow(i4_ar)

plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
