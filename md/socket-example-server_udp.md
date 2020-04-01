---
title: socket example server udp (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'server udp'


Modules used in program: 
* `import socket`

## python server udp

Python socket example: server udp

```python
import socket

# 创建实例
sk = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # ipv4, udp

# 定义IP和端口
ip_addr = ("127.0.0.1", 8888)

# 绑定监听
sk.bind(ip_addr)

# 不断循环接收数据
while True:
    data = sk.recv(1024)
    print("[Server] Recvied " + data.decode())

# 不需要关闭函数, 不会建立安全通道连接


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
