---
title: socket example py3 asyncore server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'py3 asyncore server'


Modules used in program: 
* `import threading`
* `import socket`
* `import asyncore`

## python py3 asyncore server

Python socket example: py3 asyncore server

```python
import asyncore
import socket
import threading


class ChatServer(asyncore.dispatcher):

    def __init__(self, host, port):
        asyncore.dispatcher.__init__(self)

        self._thread = None
        self._read_handler = None

        self.create_socket(socket.AF_INET, socket.SOCK_STREAM)
        self.set_reuse_addr()
        self.bind((host, port))
        self.listen(5)

    def start(self):
        """
        For use when an asyncore.loop is not already running.
        Starts a threaded loop.
        """
        if self._thread is not None:
            return

        self._thread = threading.Thread(target=asyncore.loop, kwargs={'timeout':1})
        self._thread.daemon = True
        self._thread.start()

    def stop(self):
        """Stops a threaded loop"""
        self.close()
        if self._thread is not None:
            thread, self._thread = self._thread, None 
            thread.join()

    def set_read_handler(self, read_fn):
        """
        Set a callable function that accepts a socket which is
        ready for data to be read
        """
        if not callable(read_fn):
            raise TypeError('read_fn %r is not callable' % read_fn)

        class Handler(asyncore.dispatcher_with_send):
            def handle_read(self):
                read_fn(self)

        self._read_handler = Handler

    def handle_accept(self):
        pair = self.accept()
        if pair is None:
            return

        sock, addr = pair
        if self._read_handler is None:
            print('No read handler. Refusing connection from %s' % repr(addr))
            sock.close()
            return

        print('Incoming connection from %s' % repr(addr))
        self._read_handler(sock)


class SomeReader(object):

    def __init__(self, host='localhost', port=8080):
        self.server = ChatServer(host, port)
        self.server.set_read_handler(self.handle_read)
        self.server.start()

    def handle_read(self, sock):
        data = sock.recv(8192)
        sock.send(b'ECHO: ')
        sock.send(data)


class SomeWriter(object):

    def __init__(self, host='localhost', port=8080):
        self._sock = socket.create_connection((host, port))

    def send(self, data):
        self._sock.sendall(data)


if __name__ == '__main__':
    reader = SomeReader()

    writer = SomeWriter()
    writer.send(b'hello')

    import time
    time.sleep(10)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
