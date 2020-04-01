---
title: socket example simple socket server socket server server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'simple socket server socket server server'


Modules used in program: 
* `import socket`

## python simple socket server socket server server

Python socket example: simple socket server socket server server

```python
# coding=utf-8
import socket
from vehicle_socket import *
from zUtils.util_log import log
from vehicle_socket.server.server_thread import ServerThread

id_client = 0
socket_server = None
flag = True
try:
    # bing to Host:port, using TCP
    socket_server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)   # 定义socket类型，网络通信，TCP
    socket_server.bind((HOST, PORT))   # 套接字绑定的IP与端口
    socket_server.listen(1)         # 开始TCP监听
    flag = True
    print('---**---Welcome!')
    log.info('---**---listening on {0}:{1}'.format(HOST, PORT))
except Exception, e:
    flag = False
    log.error('service failed to start! Error: {0}'.format(e.args))

try:
    while flag:
        id_client += 1
        # 接受TCP连接，并返回新的套接字与IP地址
        socket_z, address_z = socket_server.accept()
        # 开启新线程，并传递socket，id号码自增1
        server_thread = ServerThread(id_client, socket_z, address_z)
        server_thread.start()
except Exception, e:
    log.error('connection od client {0} failed! Error: {1}'.format(id_client, e.args))

finally:
    print('---**---Bye!')
    if socket_server is not None:
        socket_server.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
