---
title: socket example alice (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'alice'

Functions in program: 
* `def main():`
* `def remove_client(name):`
* `def eprint(*args, **kwargs):`

## python alice

Python socket example: alice

```python
#!/usr/bin/python3
from socket import \
    socket, \
    AF_INET, \
    SOCK_STREAM, \
    SOL_SOCKET, \
    SO_REUSEADDR
from selectors import DefaultSelector, EVENT_READ
from sys import argv, stderr
from threading import Thread
from types import SimpleNamespace
from os.path import isfile


def eprint(*args, **kwargs):
    print(file=stderr, *args, **kwargs)


clients = []


def remove_client(name):
    global clients
    clients = list(filter(lambda c: c.name != name, clients))


class Dispatcher(Thread):

    def __init__(self, arguments, selector):
        """
        Create a master socket on a specific interface and port,
        if the interface is '_' then listen on all of the available
        """
        super().__init__(daemon=True)
        if len(arguments) < 3:
            eprint('Not enough arguments\n'
                   'Usage:\n'
                   'alice interface port\n')
            raise Exception
        try:
            port = int(arguments[2])
        except ValueError:
            eprint('Non-int port is given')
            raise ValueError
        interface = arguments[1] if arguments[1] != '_' else ''
        self.selector = selector
        self.info = (interface, port)
        self.sock = socket(AF_INET, SOCK_STREAM)
        self.sock.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)  # Turn on address reuse

    def run(self):
        self.sock.bind(self.info)
        self.sock.listen()
        next_name = 1
        while True:
            con, addr = self.sock.accept()
            con.setblocking(False)
            name = 'u' + str(next_name)
            client = SimpleNamespace(name=name, current_file='', outb='')
            clients.append(client)
            self.selector.register(con, EVENT_READ, data=client)  # Available only for writing
            print(f'New CIR is accepted from {str(addr)}')


class Server(Thread):
    """
    Handle all incoming connections
    """

    def __init__(self, selector):
        super().__init__()
        self.selector = selector

    @staticmethod
    def _gen_filename(filename):
        postfix = ''
        i = 0
        while isfile(filename + postfix):
            postfix = str(i)
            i += 1
        filename += str(postfix)
        return filename

    @staticmethod
    def _write_file(filename, data):
        f = open(filename, 'ab')
        f.write(data)
        f.close()

    def _read(self, sock, client):
        data = sock.recv(1024)
        if not client.current_file and data:
            client.current_file = self._gen_filename(data.decode())
        else:
            self._write_file(client.current_file, data)

        if not data:
            self._close(sock, client)
            return

    def _close(self, sock, client):
        remove_client(client.name)
        self.selector.unregister(sock)
        sock.close()
        print(f'{client.name} disconnected')

    def run(self):
        while True:
            events = self.selector.select(timeout=None)
            for key, mask in events:
                sock = key.fileobj
                client = key.data
                if mask & EVENT_READ:
                    self._read(sock, client)


def main():
    selector = DefaultSelector()
    Server(selector).start()
    dispatcher = Dispatcher(argv, selector)
    dispatcher.run()
    dispatcher.join()
    print('All joined')  # Never goes here


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
