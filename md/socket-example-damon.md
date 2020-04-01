---
title: socket example damon (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'damon'


Modules used in program: 
* `import os`
* `import socket`
* `import time`

## python damon

Python socket example: damon

```python
#!/usr/bin/python
import time
from daemon import runner
import socket
import os

class Daemon():
    def __init__(self):
        self.stdin_path = '/dev/null'
        self.stdout_path = '/dev/tty'
        self.stderr_path = '/dev/tty'

        # Daemon specifications
        self.pidfile_path =  '/tmp/datemon.pid'
        self.pidfile_timeout = 5

        self.socket_filename = '/tmp/mysocket'

        if not os.path.exists(self.pidfile_path) and os.path.exists(self.socket_filename):
            os.remove(self.socket_filename)

    def run(self):
        while True:
            # Initialize socket
            sock = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)

            # Bind the socket to the port
            sock.bind(self.socket_filename)

            # Listen for incoming connections
            sock.listen(1)

            while True:
                # Wait for a connection
                connection, client_address = sock.accept()
                res = ""

                try:
                    # Receive the data in small chunks and retransmit it
                    while True:
                        data = connection.recv(256)
                        if data:
                            connection.sendall(data)
                            res += data
                        else:
                            break

                finally:
                    # Clean up the connection
                    connection.close()
                    print(res)

if __name__ == '__main__':
    daemon = Daemon()
    daemon_runner = runner.DaemonRunner(daemon)
    daemon_runner.do_action() 

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
