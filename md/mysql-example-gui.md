---
title: mysql example gui (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'gui'

Functions in program: 
* `def func():	`
* `def func2():	`
* `def func1():	`

Modules used in program: 
* `import mysql`
* `import intern_2W`
* `import intern_1W`

## python gui

Python mysql example: gui

```python
from Gui import *
import intern_1W
import intern_2W
import mysql
def func1():	
	s1=file.get();s2=result.get()
	intern_2W.func1(s1,s2)
def func2():	
	s1=file.get();s2=result.get()
	intern_1W.func2(s1,s2)
def func():	
	s2=result.get()
	g1=Gui()
	g1.title('CHOICE')
	g1.la('\t\t\t\t\t\t\t\t\t\t')
	g1.la('Select Your Choice:\n')
	g1.bu(text='Items Vs Delay time Analysis',command=func1)
	g1.bu(text='Plants & Items vs Delay Time Graphical Analysis',command=func2)
	g1.bu(text='MYSQL Module',command=Callable(mysql.mysql,s2))
	g1.mainloop()
g=Gui()
g.title('GUI')
g.la(text='**Kindly enter correct paths for the following.')
g.la(text='\t\t\t\t\t\t\t\t\t')
g.la(text='Enter source file location: \nExample: \'C:/Users/rohit/Desktop/Test Sample Data.xlsx\'\n')
file=g.en(text='')
g.la(text='Enter resultant file location: \nExample: \'C:/Users/rohit/Desktop/\'\n')
result=g.en(text='')
button=g.bu(text='Done',command=func)
ex=g.bu(text='Quit',command=g.quit)
g.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
