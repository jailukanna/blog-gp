---
title: socket example simple client (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'simple client'

Functions in program: 
* `def main():`
* `def myreceive(sock, MSGLEN):`

Modules used in program: 
* `import socket`
* `import codecs`
* `import sys`
* `import os`

## python simple client

Python socket example: simple client

```python
import os
import sys
import codecs
import socket

def myreceive(sock, MSGLEN):
    chunks = []
    bytes_recd = 0
    while bytes_recd < MSGLEN:
        chunk = sock.recv(min(MSGLEN - bytes_recd, 2048))
        chunks.append(chunk)
        bytes_recd = bytes_recd + len(chunk)
    return b''.join(chunks)

def main():
    # Change print(to utf-8 coding)
    sys.stdout = codecs.getwriter('utf-8')(sys.stdout.detach())

    # Create a socket object
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # Get local machine name
    host = socket.gethostname()

    # Reserve a port for your service.
    port = 7788

    s.connect((host, port))

    # Receive file list
    file_list = s.recv(2048).decode()
    print(file_list)

    while True:
        # Sent target filename
        file_name = input()
        s.send(file_name.encode())
        if file_name == '.exit':
            break

        # Receive file size
        file_size = s.recv(2048).decode()
        
        if not 'exist' in file_size:
            print('{} bytes'.format(file_size))
            # Receive file
            f = myreceive(s, int(file_size))

            if not os.path.exists('download'):
                os.mkdir('download')

            with open('download/' + file_name, 'wb') as fp:
                fp.write(f)
        else:
            print(file_size)

    # Close the socket when done
    s.close()

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
