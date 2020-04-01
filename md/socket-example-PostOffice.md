---
title: socket example PostOffice (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'PostOffice'


Modules used in program: 
* `import socket`

## python PostOffice

Python socket example: PostOffice

```python
# POST OFFICE

from queue import Queue
from threading import Lock, Thread
from time import sleep
import socket
from json import loads, dumps
from re import search


class PostOffice:
    """ PostOffice Class
        inbox (queue): threaded queue for incoming messages received
        outbox (queue): threaded queue for outgoing messages to send
        receiver (Object): threaded object that receives messages from socket and places into inbox
        sender (Object): threaded object that receives messages from outbox and sends via socket
        schema (str): string of regex expression to validate messages
    """

    def __init__(self, ip_address, port_incoming, port_outgoing):
        """ PostOffice Constructor
            Args:
                ip_address (str): The first parameter.
                i_port (int): The second parameter.
                o_port (int): The second parameter.

            Returns:
                bool: The return value. True for success, False otherwise.
        """
        # Queue's
        self.inbox = Queue()
        self.outbox = Queue()
        # Sender Worker Thread
        self.sender = Receiver(self.inbox, ip_address, port_incoming)
        self.sender.setDaemon(True)
        self.sender.start()
        # Receiver Worker Thread
        self.receiver = Sender(self.outbox, ip_address, port_outgoing)
        self.receiver.setDaemon(True)
        self.receiver.start()
        # Message Regex
        self.schema = '^\{"{from}":"{+}","{to}":"{+}","{head}":"{+}","{body}":"{*}"\}$'

    def __del__(self):
        self.inbox.join()
        self.outbox.join()
        self.sender.join()
        self.receiver.join()

    def send(self, message):
        with Lock:
            self.outbox.put(message)

    def receive(self):
        if self.inbox.empty():
            return None
        with Lock:
            message = self.inbox.get()
        self.inbox.task_done()
        return message

    def bulk_send(self, message, addresses):
        for address in addresses:
            message["to"] = address
            self.send(message)

    @staticmethod
    def compose(name, address, head, body):
        return {
            "from": name,
            "to": address,
            "head": head,
            "body": body
        }

    def is_valid_message(self, message):
        # return if message passes regex
        result = search(self.schema, message)
        if result:
            return True
        return False

    @property
    def schema(self):
        return self.schema

    @schema.setter
    def schema(self, schema):
        self.schema = schema

    def stop(self):
        self.receiver.join()
        self.sender.join()
        self.inbox.join()
        self.outbox.join()
        self.receiver = None
        self.sender = None
        self.inbox = None
        self.outbox = None


class Receiver(Thread):
    """
        Inbound Class
        -inbox queue reference
        -quit boolean
    """

    def __init__(self, inbox, ip_address, port):
        Thread.__init__(self)
        self.inbox = inbox
        self.io = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        self.io.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        self.io.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1)
        self.io.bind((ip_address, port))
        self.quit = False

    def run(self):
        while not self.quit:
            message = self.receive()  # get incoming message
            with Lock:  # lock inbox and add message
                self.inbox.put(message)
        self.io.close()

    def receive(self):
        data, address = self.io.recvfrom(1024)  # get the data and address from IO
        print('I')
        return loads(data.decode()), address  # convert the bits to ascii to json object and return with address


class Sender(Thread):
    """
        Outbound Class
        -outbox queue reference
        -quit boolean
        -delay int
    """

    def __init__(self, outbox, ip_address, port, delay=1):
        Thread.__init__(self)
        self.outbox = outbox
        self.io = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        self.io.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        self.io.setsockopt(socket.SOL_SOCKET, socket.SO_BROADCAST, 1)
        self.io.bind((ip_address, port))
        self.delay = delay
        self.quit = False

    def run(self):
        while not self.quit:
            if not self.outbox.empty():
                with Lock:
                    message = self.outbox.get()
                self.send(message)
                self.outbox.task_done()
            sleep(self.delay)
        self.io.close()

    def send(self, message):
        address = message['address']
        print('O')
        self.io.sendto(dumps(message).encode, (address[0], address[1]))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
