---
title: socket example server tcp (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'server tcp'


Modules used in program: 
* `import random`
* `import socket`

## python server tcp

Python socket example: server tcp

```python
import socket
import random
# 实例的初始化
sk = socket.socket()

# 绑定监听
ip_addr = ("127.0.0.1", 8888)
sk.bind(ip_addr)
# 最大监听数
# 此处不是并行处理数, 最大处理数是1, 所以如果是5个客户端请求, 只有1个被处理, 其他为等待状态
sk.listen(5)

# 不断循环, 接收客户端连接
while True:
    # 接收数据
    print("正在准备接收数据...")
    conn, address = sk.accept()

    msg = "连接成功"
    # 发送数据
    # Python3.x 以上, 网络数据的发送接收都是byte类型
    # 如果发送的数据为str型, 就必须进行编码
    conn.send(msg.encode())

    # 不断接收客户端发来的消息
    while True:
        # 接收客户端消息
        data = conn.recv(1024)
        # 打印数据
        print("[Server] Received msg: " + data.decode())
        # 退出, 此时的 data 为 byte 类型
        if data == b"exit":
            break
        # 发送消息回客户端
        conn.send(data)
        # 发送随机数
        conn.send(str(random.randint(0, 1000)).encode())

# 关闭连接
conn.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
