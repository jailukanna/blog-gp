---
title: socket example socketMissionFX (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socketMissionFX'

Functions in program: 
* `def missionclientthread(s):`

Modules used in program: 
* `import _thread as thread`
* `import sys`
* `import socket`

## python socketMissionFX

Python socket example: socketMissionFX

```python
#!/usr/bin/python
import socket
import sys
import _thread as thread
 
HOST = ''   # Symbolic name meaning all available interfaces
PORT = 8888 # Arbitrary non-privileged port
 
missionSocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
print(("MissionFX Socket created"))
 
#Bind socket to local host and port
try:
    missionSocket.bind((HOST, PORT))
except socket.error as msg:
    print(("Bind failed. Error Code : " + str(msg[0]) + " Message " + msg[1]))
    sys.exit()
     
print(("MissionFX Socket bind complete"))
 
#Start listening on socket
missionSocket.listen(1)
print(("MissionFX Socket now listening"))
 
#Function for handling connections. This will be used to create threads
def missionclientthread(s):

    #Sending message to connected client
    s.send(("Welcome to µBERRY.\r\n").encode())
     
    #infinite loop so that function do not terminate and thread do not end.
    #Receiving from client
    data = s.recv(1024)
    print(('Data received: ' + data.decode('utf-8')))
    reply = (b'Hello MissionFX')
    print(('Reply for sent ' , reply))
    s.sendall(reply)
    print(("Reply sent"))
	
    s.close()

#now keep talking with the client
while 1:
    #wait to accept a connection - blocking call
    conn, addr = missionSocket.accept()
    
    print(("Connected with " + addr[0] + ':' + str(addr[1])))

    #start new thread takes 1st argument as a function name to be run, 
    #second is the tuple of arguments to the function.
    thread.start_new_thread(missionclientthread, (conn,))
    
    
missionSocket.close()



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
