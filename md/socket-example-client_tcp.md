---
title: socket example client tcp (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'client tcp'


Modules used in program: 
* `import socket`

## python client tcp

Python socket example: client tcp

```python
import socket

# 实例化
client = socket.socket()

# 访问服务器端的IP和端口
ip_port = ("127.0.0.1", 8888)

# 连接服务器
client.connect(ip_port)

# 定义一个循环, 不断的发送消息
while True:
    # 接收主机信息
    data = client.recv(1024)

    # 打印接收数据
    # 如果是Python3.x, 则必须先讲数据解码
    print(data.decode())

    # 输入发送内容
    msg_input = input("请输入发送的消息: ")
    # 发送消息
    client.send(msg_input.encode())
    print(msg_input)
    if msg_input == "exit":
        break

    data = client.recv(1024)
    print("[Client] Received msg: " + data.decode())


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
