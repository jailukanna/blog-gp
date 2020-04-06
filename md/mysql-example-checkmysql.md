---
title: mysql example checkmysql (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'checkmysql'


Modules used in program: 
* `import sendgrid`
* `import mysql.connector`

## python checkmysql

Python mysql example: checkmysql

```python
import mysql.connector
import sendgrid

try:
  cnx = mysql.connector.connect(user='user', password='password',
                              host='localhost',
                              database='database')
except mysql.connector.Error as e:
  error = str(e)
  sg = sendgrid.SendGridClient('SENDGRID_API_KEY')
  
  message = sendgrid.Mail()
  message.add_to('user@focusfwd.com')
  message.set_subject('MySQL Error Detected')
  message.set_text(error)
  message.set_from('admin@focusfwd.com')
  status, msg = sg.send(message)
  print(error)
  
else:
  print('Connection successful')
  cnx.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
