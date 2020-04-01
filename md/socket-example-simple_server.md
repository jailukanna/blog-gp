---
title: socket example simple server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'simple server'

Functions in program: 
* `def main():`
* `def mysend(sock, msg):`

Modules used in program: 
* `import socket`
* `import codecs`
* `import sys`
* `import os`

## python simple server

Python socket example: simple server

```python
import os
import sys
import codecs
import socket

def mysend(sock, msg):
    totalsent = 0
    while totalsent < len(msg):
        sent = sock.send(msg[totalsent:])
        totalsent = totalsent + sent

def main():
    # Change print(to utf-8 coding)
    sys.stdout = codecs.getwriter('utf-8')(sys.stdout.detach())

    # Create a socket object
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # Get local machine name
    host = socket.gethostname()

    # Reserve a port for your service.
    port = 7788

    # Bind to the port
    s.bind((host, port))

    # Now wait for client connection.
    s.listen(1)

    while True:
        # Establish connection with client.
        client_socket, addr = s.accept()
        print('Got connection from {}'.format(addr))

        # Send file list
        file_list = [name for name in os.listdir('.') if os.path.isfile(name)]
        client_socket.send('\n'.join(file_list).encode())

        while True:
            # Receive target filename
            filename = client_socket.recv(2048).decode()
            print(filename)
            if filename == '.exit':
                break
            
            if filename in file_list:
                # Send target file size and file
                with open(filename, 'rb') as fp:
                    file = fp.read()
                    client_socket.send(str(len(file)).encode())
                    mysend(client_socket, file)
            else:
                client_socket.send('{} does not exist'.format(filename).encode())

        # Close the connection
        client_socket.close()

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
