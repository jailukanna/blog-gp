---
title: mysql example CosineSimilarity (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'CosineSimilarity'


Modules used in program: 
* `import mysql.connector`
* `import sys, getopt`

## python CosineSimilarity

Python mysql example: CosineSimilarity

```python
#!/usr/bin/python

import sys, getopt
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import mysql.connector
from mysql.connector import errorcode

SID 	= sys.argv[1]
cnx 	= mysql.connector.connect(user = 'cuser', password = 'Test*',  database = 'SPdb')
cursor 	= cnx.cursor()
query 	= ("SELECT Msg FROM ForwardMsgs WHERE SID = '%s' ORDER BY 'SentTime' DESC LIMIT 10") %SID

try:
	cursor.execute(query)
except mysql.connector.ProgrammingError as err:
	if err.errno == errorcode.ER_SYNTAX_ERROR:
		print("Check your syntax!")
		cursor.close()
		cnx.close()
		exit(1)
	else:
		print("Error: {}".format(err))

rows  = []
index = 0

for (Msg) in cursor:
	if(index > 0):
		rows.append(Msg[0].encode('utf-8'))
	index += 1

# print(rows)

index = len(rows) - 1
# print("index: " + str(index))

tfidf_vectorizer = TfidfVectorizer()
Averages = []

for rowIndex in range(index):
	# print(rowIndex)
	tfidf_matrix = tfidf_vectorizer.fit_transform(rows)
	# print(tfidf_matrix.shape)
	SimiValueNumpy = cosine_similarity(tfidf_matrix[0:1], tfidf_matrix)
	SimiValueList  =  SimiValueNumpy[0].tolist()
	SimiValueList.pop(0)
	# print(SimiValueList)
	Averages.append(sum(SimiValueList) / float(len(SimiValueList)))
	rows.pop(0)

# print(Averages)
TotalSimi = sum(Averages) / float(len(Averages))
print(TotalSimi)

cursor.close()
cnx.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
