---
title: socket example chat server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'chat server'

Functions in program: 
* `def remove(connection):`
* `def broadcast(message, connection):`
* `def clientthread(conn, addr):`

Modules used in program: 
* `import sys`
* `import select`
* `import socket`

## python chat server

Python socket example: chat server

```python
import socket
import select
import sys
from thread import *
 

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
 
IP_address = '127.0.0.1'
 
Port = 14600
 
server.bind((IP_address, Port))

server.listen(100)
 
list_of_clients = []
 
def clientthread(conn, addr):
 
   
    conn.send("Welcome to this chatroom!")
    conn.send("Please type your username")
    t=conn.recv(1024)
    t=t.rstrip('\n')
    print(t,"connected")
 
    while True:
            try:
                message = conn.recv(2048)
                if message:
 
                    
                    print("<" + t + "> " + message)
 
                  
                    message_to_send = "<" + t + "> " + message
                    broadcast(message_to_send, conn)
 
                else:
                    
                    remove(conn)
 
            except:
                continue
 

def broadcast(message, connection):
    for clients in list_of_clients:
        if clients!=connection:
            try:
                clients.send(message)
            except:
                clients.close()
 
                
                remove(clients)
 

def remove(connection):
    if connection in list_of_clients:
        list_of_clients.remove(connection)
 
while True:
 
    
    conn, addr = server.accept()

 
    
    list_of_clients.append(conn)
 
    
 
    
    start_new_thread(clientthread,(conn,addr))    
 
conn.close()
server.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
