---
title: socket example SocketThread com bean User (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'SocketThread com bean User'


Modules used in program: 
* `import socket`

## python SocketThread com bean User

Python socket example: SocketThread com bean User

```python
import socket

class User:

    def __init__(self,soc,name):
        print('初始化user信息....')
        self.soc = soc
        self.name = name
        m = soc.recv(1024)
        m = str(m,'utf-8')
        print('m = ',m)
        User.sendMsg(self,m)
        print('已发送信息 ：',m)

    def sendMsg(self,msg):
        self.soc.send(bytes(msg.encode()))

    def logout(self):
        self.soc.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
