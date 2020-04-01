---
title: socket example threaded-vim-server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'threaded-vim-server'


Modules used in program: 
* `import os`
* `import vim`
* `import time`
* `import threading`
* `import Queue`
* `import select`
* `import socket`
* `import sys`

## python threaded-vim-server

Python socket example: threaded-vim-server

```python
import sys
import socket
import select
import Queue
import threading
import time
import vim
import os

class LogError(Exception):
    pass

class Logger:
    """ Abstract class for all logger implementations.

    Concrete classes will log messages using various methods,
    e.g. write to a file.
    """

    (ERROR,INFO,DEBUG) = (0,1,2)
    TYPES = ("ERROR","Info","Debug")
    debug_level = ERROR

    def __init__(self,debug_level):
        pass

    def log(self, string, level):
        """ Log a message """
        pass

    def shutdown(self):
        """ Action to perform when closing the logger """
        pass

    def time(self):
        """ Get a nicely formatted time string """
        return time.strftime("%a %d %Y %H:%M:%S", \\
                time.localtime())

    def format(self,string,level):
        display_level = self.TYPES[level]
        """ Format the error message in a standard way """
        return "- [%s] {%s} %s" %(display_level, self.time(), str(string))


class FileLogger(Logger):
    """ Log messages to a window.

    The window object is passed in on construction, but
    only created if a message is written.
    """
    def __init__(self,debug_level,filename):
        self.filename = os.path.expanduser(filename)
        self.f = None
        self.debug_level = int(debug_level)

    def __open(self):
        try:
            self.f = open(self.filename,'w')
        except IOError as e:
            raise LogError("Invalid file name '%s' for log file: %s" \\
                    %(self.filename,str(e)))
        except:
            raise LogError("Error using file '%s' as a log file: %s" \\
                    %(self.filename,sys.exc_info()[0]))

    def shutdown(self):
        if self.f is not None:
            self.f.close()

    def log(self, string, level):
        if level > self.debug_level:
            return
        if self.f is None:
            self.__open()
        self.f.write(\\
            self.format(string,level)+"\\n")
        self.f.flush()

class QueueServer(threading.Thread):
    def __init__(self, host, port, message_queue, lock):
        self.__message_queue = message_queue
        self.__host = host
        self.__port = port
        self.__logger = FileLogger(Logger.DEBUG, "queueserver.log")
        self.__lock = lock
        threading.Thread.__init__(self)

    def log(self, message):
        self.__logger.log(message, Logger.DEBUG)

    def run(self):
        try:
            self.log("Started")
            self.log("Listening on port %s" % self.__port)
            s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            s.setblocking(0)
            s.bind((self.__host, self.__port))
            s.listen(5)
            while 1:
                try:
                    self.peek_for_exit()
                    client, address = s.accept()
                    self.log("Found client, %s" % str(address))
                    self.client_read(client)
                    while 1:
                        self.log("Waiting for message")
                        message = self.__message_queue.get()
                        self.log("Received message: %s" % message)
                        self.check_exit(message)
                        self.client_write(client, message)
                        if not self.client_read(client):
                            self.log("Closing client connection")
                            client.close()
                            break
                except socket.error, e:
                    if e.errno == 11:
                        pass
                    else:
                        if client:
                            client.close()
                        raise e
        except Exception, e:
            self.log("Error: %s" % str(sys.exc_info()))
            self.log("Stopping server")
            raise e

    def client_write(self, client, msg):
        self.log("Writing message: %s" % msg)
        client.send(msg)

    def client_read(self, client):
        self.log("Receiving")
        data = client.recv(1024)
        if data == "":
            self.log("Client connection closed")
            return False
        else:
            self.log("Received data: %s" % data)
            self.__lock.acquire()
            self.log("Acquired lock")
            vim.current.buffer.append(data.strip().split("\\n"))
            self.__lock.release()
            self.log("Written to buffer")
            return True

    def peek_for_exit(self):
        try:
            self.check_exit(self.__message_queue.get_nowait())
        except Queue.Empty:
            pass

    def check_exit(self, message):
         if message == "exit":
            raise Exception("Exiting")

class BackgroundSocketServer:
    def __init__(self):
        self.__message_queue = Queue.Queue(0)
        self.__lock = threading.Lock()
        self.__thread = None

    def __del__(self):
        self.stop()

    def start(self):
        if self.__thread:
            print(self.__thread.is_alive())

        if self.__thread and self.__thread.is_alive():
            print("Already running!")
        else:
            self.__thread = QueueServer('127.0.0.1', 9000, self.__message_queue,
self.__lock)
            self.__thread.start()
            print("Started queue server thread")

    def send_message(self, message):
        self.__message_queue.put(message)

    def stop(self):
        if self.__thread and self.__thread.is_alive():
            print("Sending exit message")
            self.__message_queue.put_nowait("exit")
            print("Joining threads")
            self.__thread.join()
            print("Stopped")
        else:
            print("Already stopped!")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
