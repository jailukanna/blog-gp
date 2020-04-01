---
title: socket example socket rate limit (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket rate limit'

Functions in program: 
* `def rate_limit_send(sender_func):`

Modules used in program: 
* `import threading`
* `import random`
* `import time`
* `import socket`

## python socket rate limit

Python socket example: socket rate limit

```python
import socket
import time
from functools import wraps

import random
import threading

def rate_limit_send(sender_func):
    @wraps(sender_func)
    def sender_wrapper(self, string, *args, **kwargs):
        if not self._rate_limit:
            return sender_func(self, string, *args, **kwargs)
        
        current_time = time.time()
        time_delta = current_time - self._last_time
        self._last_time = current_time
        self._debt = max(self._debt - time_delta*self._rate_limit, 0)
        
        if self._debt == 0:
            nbytes = sender_func(self, string, *args, **kwargs)
            if nbytes is None:
                # This is the case for the sendall function
                nbytes = len(string)
            # TODO: take into account header size
            self._debt += nbytes
            return nbytes
        else:
            sleep_period = self._debt / self._rate_limit
            print("debt:", self._debt, "sleep:", sleep_period)
            
            if self._sock.gettimeout() is not None:
                if sleep_period > self._sock.gettimeout():
                    raise socket.timeout("%s will reach timeout" % sender_func.__name__)
            
            time.sleep(sleep_period)
            
            return sender_func(self, string, *args, **kwargs)

    return sender_wrapper

class ThrottledSocket(object):
    '''
    classdocs
    '''
    def __init__(self, *args, **kwargs):
        '''
        Constructor
        '''
        self._rate_limit = kwargs.pop("rate_limit", False)
        self._last_time = time.time() 
        self._debt = 0
        
        self._sock = socket.socket(*args, **kwargs)
    
    
    def __getattr__(self, attr):
        return getattr(self._sock, attr)
    
    @rate_limit_send
    def sendto(self, *args, **kwargs):
        return self._sock.sendto(*args, **kwargs)
    
    @rate_limit_send
    def send(self, *args, **kwargs):
        return self._sock.send(*args, **kwargs)
    
    @rate_limit_send
    def sendall(self, *args, **kwargs):
        return self._sock.sendall(*args, **kwargs)
    
if __name__ == "__main__":
    SOCK_FAMILY = socket.AF_INET
    SOCK_PROTO = socket.SOCK_STREAM
    SOCK_RCV_PORT = 11111
    
    def read_from_socket(lock1, lock2):
        s = ThrottledSocket(SOCK_FAMILY, SOCK_PROTO)
        s.settimeout(1)
        s.bind(("127.0.0.1", SOCK_RCV_PORT))
        
        if SOCK_PROTO == socket.SOCK_DGRAM:
            conn = s
        elif SOCK_PROTO == socket.SOCK_STREAM:
            s.listen(1)
            lock2.release()
            conn, _ = s.accept()
            
        while not lock1.acquire(False):
            try:
                nbytes = conn.recv(random.randint(1, 1024))
                print("Received", len(nbytes), "bytes")
            except(socket.timeout):
                pass
        
        conn.close()
        try:
            s.close()
        except:
            pass
    
    lock1 = threading.Lock()
    lock1.acquire()
    lock2 = threading.Lock()
    lock2.acquire()
    
    receiver = threading.Thread(target=read_from_socket, args=(lock1, lock2))
    receiver.start()
    
    s = ThrottledSocket(SOCK_FAMILY, SOCK_PROTO, rate_limit=10000)
    s.settimeout(0.1)
    
    if SOCK_PROTO == socket.SOCK_DGRAM:
        pass
    elif SOCK_PROTO == socket.SOCK_STREAM:
        lock2.acquire()
        s.connect(("127.0.0.1", SOCK_RCV_PORT))

    for j in xrange(1000):
        try:
            #nsent = s.sendto("1"*random.randint(1, 1024), ("127.0.0.1", SOCK_RCV_PORT))
            nsent = s.sendall("1"*random.randint(1, 1024))
            print("sent", nsent, "bytes")
        except socket.timeout as e:
            print("Error in send:", e)
        
        time.sleep(random.random()/10000)
    
    s.close()
    
    lock1.release()
    receiver.join()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
