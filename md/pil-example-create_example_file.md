---
title: pil example create example file (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'create example file'

Functions in program: 
* `def createExamples():`

## python create example file

Python pil example: create example file

```python
def createExamples():
	createFile = open('exFile.txt','a')
	numberEx = range(0,10) #0.1,1.1,2.1
	numberVer = range(1,10) #0.1,0.2,0.3
	
	for eachNum in numberEx:
		for eachVer in numberVer:
			imgFilePath = 'images/numbers/'+str(eachNum)+'.'+str(eachVer)+'.png' #create image path
			i = Image.open(imgFilePath)
			iar = np.array(i)
			iar = str(iar.tolist())

			lineToWrite = str(eachNum) +'::'+iar+'\n' #generating liine to write in file
			createFile.write(lineToWrite) # writing to file

      
#createExamples() //function call



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
