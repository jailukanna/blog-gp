---
title: socket example simple socket server socket client client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'simple socket server socket client client'


Modules used in program: 
* `import socket`

## python simple socket server socket client client

Python socket example: simple socket server socket client client

```python
# coding=utf-8
import socket

from zUtils.util_log import log
from server_socket import *

s = None
try:
    # connect to server
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)      # 定义socket类型，网络通信，TCP
    s.connect((HOST, PORT))       # 要连接的IP与端口
    log.info('connected to {0}:{1}'.format(HOST, PORT))
    try:
        # 循环接收来自客户端的命令
        count = 0
        while True:
            cmd = raw_input("[client] > ")       # 与人交互，输入命令
            # cmd = 'test'
            if len(cmd) == 0:
                continue
            if cmd.lower() == 'exit':
                s.sendall('exit')
                print('Bye!')
                break
            # 想服务器端发送输入的命令
            s.sendall(cmd)
            # 获取接收的数据
            data = s.recv(1024)
            data = data.decode('GB2312').encode('utf-8')
            count += 1
            log.debug('Ask_{0}: {1}, Reply:\n{2}'.format(count, cmd, data))

    except Exception, e:
        log.error('Error: connect aborted! Args: {0}'.format(e.args))

except Exception, e:
    log.error('connect failed! Args: {0}'.format(e.args))

finally:
    if s is not None:
        s.close()   # 关闭连接
        log.info('*-----* Close Client.')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
