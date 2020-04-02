---
title: pil example find the number (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'find the number'

Functions in program: 
* `def whatNumberIsThis(imgFilePath):`

## python find the number

Python pil example: find the number

```python
def whatNumberIsThis(imgFilePath):
	matchedAr = []
	loadExmp = open('exFile.txt','r').read()
	loadExmp = loadExmp.split('\n')

	exImage = Image.open(imgFilePath)
	eIAr = np.array(exImage)
	eIAr = threshold(eIAr)
	eiArL = eIAr.tolist()
	
	imgToFind = str(eiArL)
	
	for eachExmp in loadExmp:
		if len(eachExmp) > 3 :
			eachExmp = eachExmp.split('::')
			numExmp = eachExmp[0]
			numAr = eachExmp[1]
			
			eachPix = numAr.split('],')
			eachPix_imgToFind = imgToFind.split('],')
			
			x=0;
			while(x < len(eachPix)):
				if eachPix[x] == eachPix_imgToFind[x]:
					matchedAr.append(int(numExmp))
				x += 1		
	c=Counter(matchedAr)
	print(c)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
