---
title: socket example LANconnection (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'LANconnection'


Modules used in program: 
* `import socket, time, logging`

## python LANconnection

Python socket example: LANconnection

```python
# How to use:
# LANconnect() <- starts connection,
#            - note that the server side must be started first
#
# .send(msg) <- sends a msg, this must be in bytes
# .receive(length) <- receives msg, length is the length of msg
#
# .cleanup() <- use at the end of code to close sockets

import socket, time, logging
logging.basicConfig(level=logging.INFO)

class LANconnect:

    def __init__(self,side,ip,myip=None,port=5005, blocking=True):
        '''
        Creates a LANconnect object which holds a socket object
        side specifies the type of socket and should equal either "client" or "server" 
        '''
        if not side in ["client","server"]:
            raise ValueError('side can only equal "client" or "server"')
        self.side = side
        if myip == None:
            myip = socket.gethostbyname(socket.gethostname()) 
            # on some computers this raises an exception, in that case myip has to be specified
        self.myip = myip
        self.ip = ip
        self.port = port
        self.blocking = blocking
        if self.side == "client":
            self.s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            self.s.connect((self.ip, self.port))
            logging.info('Connected')
            self.testConnection()
        if self.side == "server":
            self.serv = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            attempts = 0
            while attempts < 3:
                try:
                    self.serv.bind((myip, port))
                except OSError:
                    logging.warn('Waiting for socket to close')
                    attempts += 1
                        
                    time.sleep(60)
                else:
                    break
            else:
                raise OSError("Socket failed "+ attempts +" times, giving up")

            self.serv.listen(1)
            logging.info('Done binding server socket')
            logging.info('Waiting for connection')
            (self.s, self.p) = self.serv.accept()
            self.serv.close()
            self.testConnection()

    def testConnection(self):
        logging.info("Connected: "+str(self.s))
        logging.info("Testing connection")
        id_msg = self.side
        id_msg = id_msg.encode()
        self.s.send(id_msg)
        resp = self.s.recv(10)
        resp = resp.decode()
        if self.side == "server":
            notSide = "client"
        else:
            notSide = "server"
        if resp != notSide:
            raise ValueError('Expected "' + notSide + '", got "' + resp + '"')
        else:
            logging.info('Setup Done, Connection good')
        self.s.setblocking(self.blocking)

    def send(self, data):
        '''
        Sends data 
        '''
        if isinstance(data, str):
            data = data.encode()
        self.s.send(data)

    def receive(self, length):
        return self.s.recv(length)

    def cleanup(self):
        self.s.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
