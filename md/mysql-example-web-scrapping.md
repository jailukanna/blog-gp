---
title: mysql example web-scrapping (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'web-scrapping'


Modules used in program: 
* `import pymysql.cursors`
* `import urllib`

## python web-scrapping

Python mysql example: web-scrapping

```python
import urllib
#from BeautifulSoup import BeautifulSoup
# or if you're using BeautifulSoup4:
from bs4 import BeautifulSoup
#import mysql for python
import pymysql.cursors

connection=pymysql.connect(host="localhost",user="root",passwd="****",db="etsy")

# if you wanna save result into a file just comment the mysql lines and uncomment the file ones
#if you want more pages just change 11 with the value incremented by 1 (if 22 you must put 23)
for i in range(1,11):
	#change the URL with the website you wanna scrap
  link = str("https://www.etsy.com/fr/search?q=bois+d%26%2339%3Bolive&explicit=1&page="+str(i))
	
	
	try:

		soup = BeautifulSoup(urllib.request.urlopen(link).read(),"html.parser")
		
		#file = open("testfile.txt","a")
		#file.write("Page = {}\n".format(i))
		for price, name in zip(soup('span', {'class': 'currency-value'}),soup('p', {'class': 'text-gray-lighter'})):

			name=name.string.strip()
			price=price.string.strip().replace(",",".")
			
		
		
			try:
				with connection.cursor() as cursor:
					
					sql = "INSERT INTO `products` (`name`, `price`) VALUES (%s, %s)"
					cursor.execute(sql,(name,price))

				connection.commit()
			except Exception as inst:
				print(inst)
	
			#file.write('Name: {} ==> Price: {}\n'.format(name.string, price.string))
		#file.close()   
		
	except Exception as inst:
		print(inst)
	



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
