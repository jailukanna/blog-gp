---
title: socket example bob (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'bob'

Functions in program: 
* `def main():`
* `def eprint(*args, **argv):`

## python bob

Python socket example: bob

```python
#!/usr/bin/python3
from sys import argv, stderr
from time import sleep
from os.path import getsize
from tqdm import tqdm
from socket import \
    socket, \
    AF_INET, \
    SOCK_STREAM


def eprint(*args, **argv):
    print(file=stderr, *args, **argv)


def main():
    if len(argv) < 4:
        eprint('Not enough arguments\n'
               'Usage:\n'
               'bob file rcpt_addr rcpt_port\n')
        raise Exception
    sock = socket(AF_INET, SOCK_STREAM)
    filename, rcpt_addr, rcpt_port = argv[1:]
    rcpt_port = int(rcpt_port)
    sock.connect((rcpt_addr, rcpt_port))
    sock.send(filename.encode())
    sleep(0.001)
    file = open(filename, 'rb')
    size = getsize(filename)
    progress = tqdm(total=size, unit='bytes')
    data = True
    while data:
        data = file.read(1024)
        sleep(0.001)
        progress.update(len(data))
        sock.send(data)


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
