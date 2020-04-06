---
title: mysql example mysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mysql'

Functions in program: 
* `def mysql(path):	`

## python mysql

Python mysql example: mysql

```python
from sqlalchemy import *
from Gui import *
def mysql(path):	
	def func(uname,ps,loc,db_name):	
		engine = create_engine('mysql://'+uname.get()+':'+str(ps.get())+'@'+str(loc.get())+'/'+str(db_name.get()))
		connection=engine.connect()
		if [len(cat.get()),len(post.get()),len(entry.get()),len(plant.get())]==[0,0,0,0]:	
			g1=Gui()
			g1.title('Error')
			g1.la('\t\t Invalid Input. \t\t \nAtleast one field is required.')
			g1.mainloop()
		else:	
			t={'category':cat.get(),'post':post.get(),'entry':entry.get(),'plant':plant.get()}
			q=''
			for val in t:	
				if len(t[val])!=0: q=q+'REPLACE(LOWER('+val+')'+',\' \',\'\''+')'+'='+'\''+t[val].lower().replace(' ','')+'\''+'$'
			q=q[:len(q)-1]
			qu=q.replace('$',' AND ')
			query='SELECT category,post,entry,plant FROM intern WHERE '+qu+';'
			result=connection.execute(query)
			f=open(path+'data.txt','w')
			for val in result:	
				s=''
				for row in val: s=s+str(row)+','
				f.write(s[:len(s)-1]+'\n')
			f.close()
			g.la('File Saved at: '+path)
			connection.close()
	g=Gui()
	g.title('MYSQL')
	g.la(text='MYSQL Module\n')
	g.la(text='\t\t\t\t\t\t\t\t\t')
	g.la(text='Enter Username (For MYSQL): ')
	uname=g.en(text='')
	g.la(text='\nEnter Password (Leave empty if blank): ')
	ps=g.en(text='')
	g.la(text='\nEnter location for MYSQL server (\'localhost\' if running locally): ')
	loc=g.en(text='')
	g.la(text='\nEnter database name: ')
	db_name=g.en(text='')
	g.la(text='\nCategory: ')
	g.la(text='Valid Format: [A,B,C,D,E]\n')
	cat=g.en(text='')
	g.la(text='\nPosting Date (yyyy-mm-dd): \n')
	post=g.en(text='')
	g.la(text='\nEntry Date (yyyy-mm-dd): \n')
	entry=g.en(text='')
	g.la(text='\nPlant: \n')
	g.la(text='Valid Format: [Chain plant, chain plant, chainplant.....]\n')
	plant=g.en(text='')
	g.la(text='')
	g.bu(text='Done',command=Callable(func,uname,ps,loc,db_name))
	g.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
