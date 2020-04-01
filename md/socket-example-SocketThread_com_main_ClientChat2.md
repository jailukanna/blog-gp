---
title: socket example SocketThread com main ClientChat2 (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'SocketThread com main ClientChat2'

Functions in program: 
* `def main():`
* `def recieve_msg(username, s):`

Modules used in program: 
* `import threading, time`
* `import socket`
* `import sys`

## python SocketThread com main ClientChat2

Python socket example: SocketThread com main ClientChat2

```python
import sys
import socket
import threading, time

# global variable
isNormar = True
other_usr = ''


def recieve_msg(username, s):
    global isNormar, other_usr
    print('Please waiting other user login...')
    s.send(('login|%s' % username).encode())
    while (isNormar):
        data = s.recv(1024)  # 阻塞线程，接受消息
        data = str(data,'utf-8')
        msg = data.split('|')
        if msg[0] == 'login':
            print('%s user has already logged in, start to chat' % msg[1])
            other_usr = msg[1]
        else:
            print(msg[0])


# 程序入口
def main():
    global isNormar, other_usr
    try:
        usrname = input('Please input your name:')
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect(("127.0.0.1", 9999))
        t = threading.Thread(target=recieve_msg, args=(usrname, s))
        t.start()
    except:
        print('connection exception')
        isNormar = False
    finally:
        pass
    while isNormar:
        msg = input()  # 接受用户输入
        if msg == "exit":
            isNormar = False
        else:
            if (other_usr != ''):
                s.send(bytes(("talk|%s|%s" % (other_usr, msg)).encode()))  # 编码消息并发送
    s.close()


if __name__ == "__main__":
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
