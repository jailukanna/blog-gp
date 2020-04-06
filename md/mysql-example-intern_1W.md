---
title: mysql example intern 1W (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'intern 1W'

Functions in program: 
* `def func2(path,file_path):	`

## python intern 1W

Python mysql example: intern 1W

```python
def func2(path,file_path):	
	import Gui
	import openpyxl
	import numpy as np
	import matplotlib.pyplot as plt
	try:	
		wb=openpyxl.load_workbook(path)
		sheet=wb.get_sheet_by_name('Sheet1')
		d={'a_c':0L,'a_ch':0L,'a_h':0L,'a_b':0L,'a_l':0L,'b_c':0L,'b_ch':0L,'b_h':0L,'b_b':0L,'b_l':0L,'c_c':0L,'c_ch':0L,'c_h':0L,'c_b':0L,'c_l':0L,'d_c':0L,'d_ch':0L,'d_h':0L,'d_b':0L,'d_l':0L,'e_c':0L,'e_ch':0L,'e_h':0L,'e_b':0L,'e_l':0L}
		cap=['A','B','C','D','E'];low=['a','b','c','d','e']
		for i in range(5):	
			for row in tuple(sheet['A2':(sheet.cell(row=sheet.max_row,column=sheet.max_column)).coordinate]):	
				if str(row[0].value)==cap[i]:	
					if str(row[4].value)=='Chain Plant':	
						if (((row[3].value-row[1].value).total_seconds())/86400)>2: d[low[i]+'_c']+=1
					elif str(row[4].value)=='Chennai Plant':	
						if (((row[3].value-row[1].value).total_seconds())/86400)>2: d[low[i]+'_ch']+=1
					elif str(row[4].value)=='Haridwar Plant':	
						if (((row[3].value-row[1].value).total_seconds())/86400)>2: d[low[i]+'_h']+=1
					elif str(row[4].value)=='Bawal Plant':	
						if (((row[3].value-row[1].value).total_seconds())/86400)>2: d[low[i]+'_b']+=1
					elif str(row[4].value)=='Ludhiana Plant':	
						if (((row[3].value-row[1].value).total_seconds())/86400)>2: d[low[i]+'_l']+=1
		X=np.array([0,4,8,12,16])
		gap=0.5
		fig,ax = plt.subplots()
		ax.set_xticks(X+(gap+2)/2)
		ax.set_xticklabels(('Chain\nPlant', 'Chennai\nPlant','Haridwar\nPlant', 'Bawal\nPlant', 'Ludhiana\nPlant'))
		p1=ax.bar(X,[d['a_c'],d['a_ch'],d['a_h'],d['a_b'],d['a_l']],color='crimson',width=gap)
		p2=ax.bar(X+gap,[d['b_c'],d['b_ch'],d['b_h'],d['b_b'],d['b_l']],color='palegreen',width=gap)
		p3=ax.bar(X+(2*gap),[d['c_c'],d['c_ch'],d['c_h'],d['c_b'],d['c_l']],color='lightcoral',width=gap)
		p4=ax.bar(X+(3*gap),[d['d_c'],d['d_ch'],d['d_h'],d['d_b'],d['d_l']],color='gold',width=gap)
		p5=ax.bar(X+(4*gap),[d['e_c'],d['e_ch'],d['e_h'],d['e_b'],d['e_l']],color='steelblue',width=gap)
		ax.legend((p1,p2,p3,p4,p5),('A','B','C','D','E'))
		plt.ylim((0,13000))
		plt.xlabel('Location')
		plt.ylabel('No. of cases with Delay > 2 Days')
		plt.savefig(file_path+'Graph.png')
		plt.show()
	except IOError: g=Gui.Gui();g.title('Error');g.la(text='Invalid Path/No such file exists.');g.mainloop()
	except: g=Gui.Gui();g.title('Error');g.la(text='Something went wrong. Exiting..');g.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
