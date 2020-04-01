---
title: socket example day1 socket1 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'day1 socket1'


Modules used in program: 
* `import socket`

## python day1 socket1

Python socket example: day1 socket1

```python
import socket
udp_socket = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
test_addr = ("192.168.36.0",8080)
send_test = input("请输入要发送的信息")
udp_socket.sendto(send_test.encode("utf-8"),test_addr)

udp_socket.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
