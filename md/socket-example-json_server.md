---
title: socket example json server (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'json server'


Modules used in program: 
* `import yaml`
* `import threading`
* `import traceback`
* `import SocketServer`
* `import operator`
* `import json`
* `import argparse`

## python json server

Python socket example: json server

```python
import argparse
import json
import operator
import SocketServer
import traceback
import threading
import yaml

from time import strftime, localtime



class MyTCPServer(SocketServer.ThreadingTCPServer):
    allow_reuse_address = True

class MyTCPServerHandler(SocketServer.BaseRequestHandler):

    def handle(self):

        response_template = '''HTTP/1.1 200 OK
Content-Type: text/plain
Access-Control-Allow-Origin: *\r\n\r\n%s
        '''

        try:
            # Handle HTTP Requests
            data = self.request.recv(2048).strip()
            print("Data", data)

            # Extract content
            json_data = json.loads(data.split('\r\n\r\n')[-1])

            # Get response
            response = "Response from server"

        except Exception, e:
            print("Exception while receiving message: ",e)
            traceback.print_exc()
            response = {"err": "invalid message format"}

        finally:
            # send back response
            response = response_template%(str(response))
            print("Response ", response)

            '''
            # For checking Performance
            self.server.c.prnt(("Performance output "),'r')
            self.server.c.prnt((self.server.h.heap()),'g')
            '''
            self.request.sendall(response)


if __name__ == "__main__":

    # Init Variables
    server = MyTCPServer(('127.0.0.1', 13333), MyTCPServerHandler)

    print("Manoj is awesome \nYou agree to this by running this server..")
    server.serve_forever()

    # Start a thread with the server -- that thread will then start one
    # more thread for each request
    server_thread = threading.Thread(target=server.serve_forever)

    # Exit the server thread when the main thread terminates
    server_thread.daemon = True
    server_thread.start()
    server.shutdown()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
