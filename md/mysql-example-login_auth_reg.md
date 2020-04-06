---
title: mysql example login auth reg (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'login auth reg'

Functions in program: 
* `def searchuser_password():`
* `def login(usrname,passwd):`
* `def register( user_idf,name,username,passwd):`

Modules used in program: 
* `import mysql.connector`

## python login auth reg

Python mysql example: login auth reg

```python
import mysql.connector

# Put here your database details 
user_db=mysql.connector.connect(
    host="hostname",
    user="username",
    passwd="*********",
    database="database_name"

)
user_details=user_db.cursor()

#Used to register new user to the system
def register( user_idf,name,username,passwd):
    
    #using sql commands in python
    sql="INSERT INTO user_info.user (user_id,name,username,passwod) VALUES (%s, %s, %s, %s)"
    val=(user_idf,name,username,passwd)
    
    try:
        user_details.execute(sql,val)
        user_db.commit()
        return 0
    except:
        return 1
      
#Used for login system 
def login(usrname,passwd):

    user_details.execute("SELECT username,passwod FROM user_info.user")
    result=user_details.fetchall()
    chk=0
    for i in range(len(result)):
        temp=[]
        temp=result[i]
        
        if temp[0]==usrname and temp[1]==passwd:
            chk=chk+1
        else:
            pass
    if chk>0:
        return 0
    else:
        return 1
      
#used to search user in the database
def searchuser_password():

    user_details.execute("SELECT username,passwod FROM user_info.user")
    userinfo=user_details.fetchall()
    return userinfo

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
