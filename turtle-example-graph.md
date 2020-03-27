---
title: turtle example graph (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'graph'

Functions in program: 
* `def main():`

## python graph

Python turtle example: graph

```python
#!/usr/bin/python

from turtle import *


def main():
	dataSet=[]
	outputDataSet=[[]]*10
	outputBuffer=[""]*10
	with open("datafile.txt") as fileInput:
		for line in fileInput:
			numbers_int=map(int,line.split())
			dataSet.extend(numbers_int)
	dataSet.sort()
	print(str(dataSet)+'\n')
	for data in dataSet:
		outputDataSet[data//10]=outputDataSet[data//10]+[data%10];
	up()
	goto(-400,300)
	right(90)

	for i in range(10):
		outputBuffer[i]=str(i)+' | '
		for j in outputDataSet[i]:
			outputBuffer[i]+=str(j)+' '
		write(outputBuffer[i], False , align="left",font=("Arial",20, "normal"))
		forward(30)

	left(90)
	forward(200)
	pos=position()
	down()
	forward(450)
	goto(pos)
	left(90)
	forward(200)
	goto(pos)
	right(90)

	for i in range(10):
		forward(40)
		pos=position()
		up()
		right(90)
		forward(20)
		write(str(i), False , align="center",font=("Arial",10, "normal"))
		goto(pos)
		down()
		if len(outputDataSet[i])>0 :
			left(180)
			forward(80*len(outputDataSet[i]))
			write(len(outputDataSet[i]), False , align="center",font=("Arial",10, "normal"))
			goto(pos)
			right(90)
		else:
			left(90)
	
	ht()
	#print(outputBuffer)
	#print(outputDataSet)


if __name__ == '__main__':
	main()
	mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
