---
title: mysql example chatrooms server (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'chatrooms server'

Functions in program: 
* `def new_room(room_name,idt):`
* `def all_rooms():`

Modules used in program: 
* `import mysql.connector`

## python chatrooms server

Python mysql example: chatrooms server

```python
import mysql.connector

#Your MySQL Database details
chatroom_serv=mysql.connector.connect(
    host="hostname",
    user="user",
    passwd="***********",
    database="databasename"

)

rooms=chatroom_serv.cursor()

#Returns the list of chatrooms registered
def all_rooms():
    room_list=[]
    
    #executing MySQL Command from Python
    rooms.execute("SELECT server_rooms FROM user_info.chatserver")
    chat_rooms_list=rooms.fetchall()
    for i in range(len(chat_rooms_list)):
        room_list.append(chat_rooms_list[i][0])
    return room_list

#To register a new chatroom
def new_room(room_name,idt):
    sql= "INSERT INTO user_info.chatserver (server_rooms,id) VALUE (%s,%s)"
    val=(room_name,idt)

    rooms.execute(sql,val)
    chatroom_serv.commit()
    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
