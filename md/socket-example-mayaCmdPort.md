---
title: socket example mayaCmdPort (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'mayaCmdPort'

Functions in program: 
* `def SendCommand():`

Modules used in program: 
* `import socket`

## python mayaCmdPort

Python socket example: mayaCmdPort

```python
'''
Command port
http://forums.cgsociety.org/archive/index.php?t-966699.html
Creative Crash
'''
import socket
#HOST = '192.168.1.122' # The remote host
HOST = '127.0.0.1' # the local host
PORT = 54321 # The same port as used by the server
ADDR=(HOST,PORT)

def SendCommand():
  client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  client.connect(ADDR)
  cmd = 'mc.polyCube()' # the commang from external editor to maya
  
  MyMessage = 'python("import maya.cmds as mc;{0}")'.format(cmd)
  client.send(MyMessage)
  data = client.recv(1024) #receive the result info
  client.close()
  
  print('The Result is %s'%data)

if __name__=='__main__':
  SendCommand()

## In Maya
#mc.commandPort(n=':54321', eo=True)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
