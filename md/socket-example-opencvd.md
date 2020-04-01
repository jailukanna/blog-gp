---
title: socket example opencvd (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'opencvd'

Functions in program: 
* `def handle(socket, address):`
* `def simple_server():`

Modules used in program: 
* `import time`
* `import gevent`
* `import cv2`

## python opencvd

Python socket example: opencvd

```python
#!/usr/bin/env python

import cv2
import gevent
import time
from ticket import ticket
from socket import *
from gevent import Greenlet
from gevent.threadpool import ThreadPool
from gevent.select import select as gselect
from gevent.server import StreamServer

RT = 0.5
FRAME_WIDTH = 2560*RT
FRAME_HEIGHT = 1440*RT


class MyGreenlet(Greenlet):

    def __init__(self, message):
        Greenlet.__init__(self)
        self.message = message

    def _run(self):
        while 1:
            print(self.message)
            gevent.sleep(5)


# subclass Greenlet class
class MyOpencv(Greenlet):

    def __init__(self, vc):
        Greenlet.__init__(self)
        self.vc = vc
        self.t = ticket()

    def _run(self):
        while True:
            _, self.frame = self.vc.read()
            self._showTLText(str(self.t.fps()))

            cv2.imshow('Video', self.frame)

            if cv2.waitKey(1) & 0xFF == ord('q'):
                print("Quit")
                break
            gevent.sleep(0.001)

        self.vc.release()
        cv2.destroyAllWindows()

    def _showTLText(self, buf):
        cv2.putText(self.frame, buf, (0,30), cv2.FONT_HERSHEY_SIMPLEX, 1,(255,5,5),2,cv2.LINE_AA)

def simple_server():
    print("server starting")
    sock = socket()
    sock.setsockopt(SOL_SOCKET, SO_REUSEADDR,1)
    sock.setblocking(0)
    sock.bind(('',6666))
    sock.listen(5)
    inputs = [sock]
    while True:
        print("1")
        read_sockets,_,_ = gselect(inputs,[],[])
        for s in read_sockets:
            print("2")
            if s is sock:
                print("3")
                client, addr = s.accept()
                print('Connection:{}'.format(addr))
                client.setblocking(0)
                inputs.append(client)
            else:
                data = s.recv(100)
                if len(data) > 0:
                    print('recv:{}'.format(data))
                else:
                    inputs.remove(s)
                    s.close()
        gevent.sleep(0)


def handle(socket, address):
    print('New connection from:{}'.format(address))
    rfileobj = socket.makefile(mode='rb')
    while True:
        line = rfileobj.readline()
        if not line:
            print("client disconnected")
            break
        print("echoed %r" % line)
    rfileobj.close()


if __name__ == '__main__':

    video_capture = cv2.VideoCapture(0)
    video_capture.set(cv2.CAP_PROP_FRAME_WIDTH, FRAME_WIDTH)
    video_capture.set(cv2.CAP_PROP_FRAME_HEIGHT, FRAME_HEIGHT)

    print('Starting opencv server on port 6666')

    server = StreamServer(('', 6666), handle)
    server.start()

    g1 = MyOpencv(video_capture)
    g1.start()

    gevent.wait()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
