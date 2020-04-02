---
title: pil example sqconnect (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'sqconnect'

Functions in program: 
* `def testReadOne(TestPhoto,recordId):`
* `def testReadMany(TestPhoto):`
* `def testInMemory():      #this function not work`
* `def testUpdateOne(TestPhoto,recordId):         #better way`
* `def testThreadUpdate(TestPhoto):`
* `def testWriteOne(TestPhoto):							#normal way use ByteIO`
* `def testThreadWrite(TestPhoto):`
* `def showMasterDB():`
* `def dropTable():`
* `def createTable():`
* `def pil_to_ui_image(img):`

Modules used in program: 
* `import threading`
* `import ui`
* `import io`
* `import sqlite3`

## python sqconnect

Python pil example: sqconnect

```python
import sqlite3
import io
import ui
from PIL import Image
import threading

from debughelper import logger

DBNAME='example.db'
TESTFILE='temp.png'
THUMBSIZE=(320,240)

class TestPhoto:
		def __init__(self):
			self.name='unknown'
			self.date=None         # no date type in sqlite
			self.mainImage=None
			self.fileKey1=None      # for upload file test
			self.recordId=0					# prime key
			
def pil_to_ui_image(img):
	bio = io.BytesIO()
	img.save(bio, 'PNG')
	data = bio.getvalue()
	bio.close()
	return ui.Image.from_data(data)

def createTable():
	conn = sqlite3.connect(DBNAME)     
	c = conn.cursor()
	c.execute('CREATE TABLE TestPhotos\
	             (recordId INTEGER PRIMARY KEY, date text, name text,  fileKey1 text, mainImage blob)')
	conn.commit()
	conn.close()

def dropTable():
	conn = sqlite3.connect(DBNAME)
	c = conn.cursor()
	c.execute('drop table TestPhotos')
	conn.commit()
	conn.close()

def showMasterDB():
	conn = sqlite3.connect(DBNAME)
	c = conn.cursor()
	c.execute('select * from sqlite_master ')
	li=c.fetchall()
	logger.debug(li)
		
def testThreadWrite(TestPhoto):
	th= threading.Thread(target=testWriteOne, args=(TestPhoto,))
	th.start()
	logger.debug('parent end')

def testWriteOne(TestPhoto):							#normal way use ByteIO
	conn = sqlite3.connect(DBNAME)
	c = conn.cursor()
	logger.debug('start')
	
	if localTest==True:
		im=Image.open(TESTFILE)
	else:
		im=TestPhoto.mainImage
	
	bio = io.BytesIO()
	im.save(bio,'PNG')							#slow s-l-o-w!
	data=bio.getvalue()
	logger.debug('prepare data finish')
	tonk=[('tgtg','name','yyyy',data)]
	try:
		r=c.executemany('INSERT INTO TestPhotos (date,name,fileKey1,mainImage) VALUES (?,?,?,?)', tonk)
		conn.commit()
		logger.debug('commit end')
	except:
		logger.error('sqlite3 error')
	conn.close()
	
def testThreadUpdate(TestPhoto):
	th= threading.Thread(target=testUpdateOne, args=(TestPhoto,))
	th.start()
	logger.debug('parent end')
	
def testUpdateOne(TestPhoto,recordId):         #better way
	conn = sqlite3.connect(DBNAME)
	c = conn.cursor()
	
	if localTest==True:
		TestPhoto.mainImage=Image.open(TESTFILE)

	im=TestPhoto.mainImage
	logger.debug('test start')
	bio = io.BytesIO()
	im.save(bio,'PNG')							#slow s-l-o-w!
	data=bio.getvalue()
	#TestPhoto.recordId+=1
	tonk=[('tgtg','mama','yyyy',data,recordId)]

	logger.debug('prepare data finish')
	try:
		r=c.executemany('''UPDATE TestPhotos set date=?,name=?,trueImage=?,mainImage=? where recordId=?''', tonk)
		conn.commit()
	except:
		logger.error('sqlite3 error')	
	logger.debug('commit finish')
	conn.close()
	
def testInMemory():      #this function not work
	logger.debug('start')
	im=Image.open(TESTFILE)
	imsize=im.size
	logger.debug('open finish')	
	
	data=im.tostring()
	logger.debug('im.save finish')
	logger.debug(im.mode)
	logger.debug(im.format)
	try:
		img = Image.fromstring('RGB', imgSize, data)  # this crash system
	except:
		logger.error('fromstring error')
	
	#logger.debug(img)
	
def testReadMany(TestPhoto):
	conn = sqlite3.connect(DBNAME)
	c = conn.cursor()
	logger.debug('start')
	c.execute('SELECT count(*) FROM TestPhotos' )
	cnt=c.fetchone()[0]
	logger.debug('count== '+str(cnt))
	
	c.execute('SELECT recordId FROM TestPhotos')
	recordIds=c.fetchall()
	for recordId in recordIds:
		#bld=TestPhoto()
		testReadOne(TestPhoto,recordId[0])
		#here we need to add bld to list
		
	conn.close()

def testReadOne(TestPhoto,recordId):
	conn = sqlite3.connect(DBNAME)    
	c = conn.cursor()
	logger.debug('start')
	logger.debug(recordId)
	c.execute('''SELECT name,date,mainImage 
							FROM TestPhotos Where recordId=?''',(recordId,))
	li=c.fetchall()
	TestPhoto.date=li[0][0]
	TestPhoto.name=li[0][1]
	logger.debug('select finish')
	logger.debug(TestPhoto.date)
	logger.debug(TestPhoto.name)
	image=li[0][2]
	#bb=ui.Image.from_data(image)      #this method fast 
	TestPhoto.mainImage=Image.open(io.BytesIO(image))
	#logger.debug(TestPhoto.mainImage)
	#bb=TestPhoto.mainImage.copy()
	#logger.debug(bb)
	#bb.thumbnail(THUMBSIZE)
	#TestPhoto.thumbImage=pil_to_ui_image(bb)
	
	logger.debug('finish read')
	conn.close()

localTest=False

if __name__ == '__main__':
	localTest=True
	testPhoto=TestPhoto()
	
	while(True):
		print('c create table')
		print('d drop table')
		print('w write data')
		print('t test wite using threading')
		print('r read data')
		print('u update data')
		print('i in memory test bug bug...')
		print('q quit')
		w=input()
		if (w=='c'):
			createTable()
			print('table created')
		elif(w=='d'):
			dropTable()
			print('table dropped')
		elif(w=='w'):
			testWriteOne(testPhoto)
		elif(w=='t'):
			testThreadWrite(testPhoto)
		elif(w=='r'):
			testReadMany(testPhoto)
		elif(w=='i'):
			testInMemory()
		elif(w=='u'):
			testUpdateOne(testPhoto,1)
		elif(w=='q'):
			break
		
	showMasterDB()
	print('enjoy')
	


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
