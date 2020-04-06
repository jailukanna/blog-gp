---
title: mysql example 5-vision to db (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example '5-vision to db'


Modules used in program: 
* `import mysql.connector`
* `import pprint`
* `import os`
* `import io`

## python 5-vision to db

Python mysql example: 5-vision to db

```python
import io
import os
import pprint
from lxml import etree
from collections import defaultdict

# Imports the Google Cloud client library
from google.cloud import vision
from google.cloud import language
import mysql.connector


#This section creates two sessions with the MySQL database
cnx = mysql.connector.connect(user='root', password='YOUR MYSQL PASSWORD',
                              host='127.0.0.1',
                              database='ocr')
cnx2 = mysql.connector.connect(user='root', password='YOUR MYSQL PASSWORD',
                               host='127.0.0.1',
                               database='ocr')


cursor = cnx.cursor(buffered=True)
cursor2 = cnx2.cursor(buffered=True)


## VISION API SECTION
# Instantiates a client
vision_client = vision.Client.from_service_account_json('PATH TO YOUR JSON KEY')

# Instantiates a client
language_client = language.Client()

#this finds the last entry in the database and sets the start of the loop at that file
files_list = []
for fn in os.listdir('PATH TO FILES FOR OCR'):
    files_list.append(fn)

files_list.sort()
end = len(files_list) 
#Get filename of last record in the db
query = ("SELECT doc_id, doc_name FROM ocr.documents WHERE doc_id = (SELECT MAX(doc_id) FROM ocr.documents);")
cursor.execute(query)

#This finds the index for the file in the list of filenames 

for doc in cursor:
    get_this = doc[1]
    print(doc[1])

try:
    i = files_list.index(get_this)
except (NameError):
    i = 1
    
for file in files_list[i:end]:

    file_name = "PATH TO FILES FOR OCR" + file

    file_name_1 = '"' + str(file) + '"'
    #file_name_2 = str(file_name.split('/', -1)[-1])

    # Loads the image into memory
    with io.open(file_name, 'rb') as image_file:
        content = image_file.read()
        image = vision_client.image(
            content=content)

        # Performs label detection on the image file
        labels = image.detect_text()
        
        #labels[0] is the whole text, all later list items are individual word tokens
        nlp = '"' + labels[0].description.encode('utf8') + '"'
        
        #This query writes the entire text result to the texts table.  This can be useful, but often has incorrect line breaks.    
        text_query = ("INSERT ocr.texts (file_name, text) VALUES (%s,%s);" % (file_name_1, nlp)) 
        try:       
            cursor.execute(text_query)
            cnx.commit()   
        except:
            pass

        for label in labels[1:]:
            print(label._description)
    
            word_1 = label._description
            if '"' in word_1:
                word_1.replace('"','\"')
    
            else:
                word = '"' + word_1 + '"'
                text = language_client.document_from_text(word_1)
            
            #This is the Natural Language API call, it will add entity type to each word. 
            entity_response = text.analyze_entities()
            try: 
              entity_type_1 = entity_response.entities[0].entity_type 
              entity_type = '"' + entity_type_1 + '"'        
            except (IndexError):
              entity_type = '"' + '"'
        
            x0 = label.bounds.vertices[0].x_coordinate
            y0 = label.bounds.vertices[0].y_coordinate
            
            x1 = label.bounds.vertices[1].x_coordinate
            y1 = label.bounds.vertices[1].y_coordinate
            
            x2 = label.bounds.vertices[2].x_coordinate
            y2 = label.bounds.vertices[2].y_coordinate
            
            x3 = label.bounds.vertices[3].x_coordinate
            y3 = label.bounds.vertices[3].y_coordinate    
            
            try:
                doc_name_query = ("INSERT ocr.words (doc_name,word,entity_type,x0,y0,x1,y1,x2,y2,x3,y3) VALUES (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s);" % (file_name_1,word,entity_type,x0,y0,x1,y1,x2,y2,x3,y3)) 

                cursor.execute(doc_name_query)
                cnx.commit()
                
            except:
                doc_name_query = ("INSERT ocr.words (doc_name,word,x0,y0,x1,y1,x2,y2,x3,y3) VALUES (%s,%s,%s,%s,%s,%s,%s,%s,%s,%s);" % (file_name_1,word,x0,y0,x1,y1,x2,y2,x3,y3)) 
                cursor.execute(doc_name_query)
                cnx.commit()                
            


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
