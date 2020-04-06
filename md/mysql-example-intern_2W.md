---
title: mysql example intern 2W (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'intern 2W'

Functions in program: 
* `def func1(path,file_path):	`

## python intern 2W

Python mysql example: intern 2W

```python
def func1(path,file_path):	
	import Gui
	import os
	import openpyxl
	import numpy as np
	try:	
		wb=openpyxl.load_workbook(path)
		sheet=wb.get_sheet_by_name('Sheet1')
		A=[]
		B=[]
		C=[]
		D=[]
		E=[]
		for row in tuple(sheet['A2':(sheet.cell(row=sheet.max_row,column=sheet.max_column)).coordinate]): 
		     if str(row[0].value)=='A': A.append(((row[3].value-row[1].value).total_seconds())/3600)
		     elif str(row[0].value)=='B': B.append(((row[3].value-row[1].value).total_seconds())/3600)
		     elif str(row[0].value)=='C': C.append(((row[3].value-row[1].value).total_seconds())/3600)
		     elif str(row[0].value)=='D': D.append(((row[3].value-row[1].value).total_seconds())/3600)
		     elif str(row[0].value)=='E': E.append(((row[3].value-row[1].value).total_seconds())/3600)
		fout=open(file_path+'analysis.txt','w')
		for letter in ['A','B','C','D','E']:
		 fout.write('\t'+letter+'\n')
		 fout.write('|-----------------------------|'+'\n')
		 fout.write('| Max Delay Time(hrs):        |'+'  '+str(max(eval(letter)))+'\n')
		 fout.write('|-----------------------------|'+'\n')
		 fout.write('| Min Delay Time(hrs):        |'+'  '+str(min(eval(letter)))+'\n')
		 fout.write('|-----------------------------|'+'\n')
		 fout.write('| Avg Delay Time(hrs):        |'+'  '+str(float(sum(eval(letter)))/len(eval(letter)))+'\n')
		 fout.write('|-----------------------------|'+'\n')
		 fout.write('| Std Dev. Delay Time(hrs):   |'+'  '+str(np.var(eval(letter))**0.5)+'\n')
		 fout.write('|-----------------------------|'+'\n\n')
		fout.close()
		os.system(file_path+'analysis.txt')
	except IOError: g=Gui.Gui();g.title('Error');g.la(text='Invalid Path/No such file exists.');g.mainloop()
	except: g=Gui.Gui();g.title('Error');g.la(text='Something went wrong. Exiting..');g.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
