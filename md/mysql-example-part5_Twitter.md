---
title: mysql example part5 Twitter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'part5 Twitter'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import nltk`
* `import pandas as pd `
* `import re`
* `import os`
* `import mysql.connector `

## python part5 Twitter

Python mysql example: part5 Twitter

```python
import mysql.connector 
from mysql.connector import Error
import os
import re
import pandas as pd 
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk.stem.porter import PorterStemmer
from nltk.stem import WordNetLemmatizer
import nltk
from wordcloud import WordCloud, STOPWORDS
import numpy as np
import matplotlib.pyplot as plt
from textblob import TextBlob


class TweetObject():
	
	def __init__(self, host, database, user):
		self.password = os.environ['PASSWORD']
		self.host = host
		self.database = database
		self.user = user
		


	def MySQLConnect(self,query):
		"""
		Connects to database and extracts
		raw tweets and any other columns we
		need
		Parameters:
		----------------
		arg1: string: SQL query
		Returns: pandas dataframr
		----------------
		"""

		try:
			con = mysql.connector.connect(host = self.host, database = self.database, \
				user = self.user, password = self.password, charset = 'utf8')

			if con.is_connected():
				print("Successfully connected to database")

				cursor = con.cursor()
				query = query
				cursor.execute(query)

				data = cursor.fetchall()
				# store in dataframe
				df = pd.DataFrame(data,columns = ['date', 'tweet'])
				#print(df.head())



		except Error as e:
			print(e)
		
		cursor.close()
		con.close()

		
		return df


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
