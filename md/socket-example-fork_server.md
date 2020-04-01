---
title: socket example fork server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'fork server'

Functions in program: 
* `def accept(descr):`

Modules used in program: 
* `import time`
* `import socket`
* `import os`

## python fork server

Python socket example: fork server

```python
import os
import socket
import time

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM, 0)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

s.bind(('0.0.0.0', 1234))
s.listen(10)


def accept(descr):
    (c, addr) = s.accept()
    print('connection got in', descr, 'from', addr)
    c.send('hello world\n')
    c.close()


print(" [*] listening on :1234, next three connections will be handled by parent")

for i in range(3):
    accept('parent')

print(" [*] good. forking now")
child_pid = os.fork()
if child_pid == 0:
    # child
    print(" [*] child: okay, accepting now!")
    while True:
        accept('child')

else:
    # parent
    print(" [*] parent: okay, accepting next three connections")
    for i in range(3):
        accept('parent')
    print(" [*] parent: I'm done, waiting few sec")
    time.sleep(5)
    print(" [*] parent: closing bind fd")
    s.close()
    print(" [*] parent: waiting again")
    time.sleep(5)
    print(" [*] parent: quitting, remember to kill the child! run:")
    print("kill ", child_pid)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
