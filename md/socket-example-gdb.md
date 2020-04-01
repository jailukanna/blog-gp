---
title: socket example gdb (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'gdb'


Modules used in program: 
* `import socket,select`

## python gdb

Python socket example: gdb

```python
import socket,select
class gdb(object):
    """description of class"""
    def __init__(self, **kwargs):
        self.connection=socket.socket()
        self.connection.connect(('127.0.0.1',55555))
        self.connection.send('+'.encode('ascii'))

    def has_data(self):
        return self.connection in select.select([self.connection],[],[],0)[0]

    def send(self, string):
        in_data=''.join(string.split(' ')).encode('ascii')
        prefix='$'.encode('ascii')
        postfix='#'.encode('ascii')

        hstr=hex(sum(in_data))
        if len(hstr)<4:
            hstr='0x0'+hstr[:1]
        checksum=hstr[-2:].encode('ascii')
        packet=prefix+in_data+postfix+checksum
        while len(packet)>0:
            packet=packet[self.connection.send(packet):]


    def recv_msg(self,full_msg=True):
        state=0
        buffer=''
        while True:
            c = self.connection.recv(1).decode('ascii')
            #print('state={},c={},buffer={}'.format(state,c,buffer))
            if state == 0:
                state = 1
                if c == '+':
                   if full_msg:
                       continue
                elif c == '-':
                    return '[ERR] Last msg resend pls'

            if state == 1:
                if c == '$':
                    state = 2
                else:
                    return '[ERR] UNKNOWN c={} in state 1'.format(c)
            elif state == 2:
                if c == '#':
                    state = 3
                else:
                    buffer += c
            elif state == 3:
                vba_csum=self.connection.recv(1).decode('ascii')
                my_csum=hex(sum(buffer.encode('ascii')))[-2:]
                self.connection.send('+'.encode('ascii'))
                return buffer
            else:
                raise Exception()

    def step(self, address=None):
        if address == None:
            self.send('s')
        else:
            self.send('s '+hex(address))[2:]
    def dump_registers(self):
        self.send('g')
    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
