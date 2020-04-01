---
title: socket example SERVER-ip updater (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'SERVER-ip updater'


Modules used in program: 
* `import subprocess                                                                                                                                                                                                                            `
* `import logging                                                                                                                                                                                                                               `
* `import socket                                                                                                                                                                                                                                `

## python SERVER-ip updater

Python socket example: SERVER-ip updater

```python
#!/usr/bin/python
#modified from https://gist.github.com/drmalex07/333d8a88c4918954e8e4
                                                                                                                                                                                                                          
from SocketServer import TCPServer, StreamRequestHandler                                                                                                                                                                                     
import socket                                                                                                                                                                                                                                
import logging                                                                                                                                                                                                                               
import subprocess                                                                                                                                                                                                                            
                                                                                                                                                                                                                                             
class Handler(StreamRequestHandler):                                                                                                                                                                                                         
    def handle(self):                                                                                                                                                                                                                        
        self.data = self.rfile.readline().strip()                                                                                                                                                                                            
        logging.info("From <%s>: %s" % (self.client_address, self.data))                                                                                                                                                                     
        subprocess.call(["/opt/ip-updater/ip_updater.sh", self.data, self.client_address[0]])                                                                                                                                                
                                                                                                                                                                                                                                             
class Server(TCPServer):                                                                                                                                                                                                                     
                                                                                                                                                                                                                                             
    SYSTEMD_FIRST_SOCKET_FD = 3                                                                                                                                                                                                              
                                                                                                                                                                                                                                             
    def __init__(self, server_address, handler_cls):                                                                                                                                                                                         
        # Invoke base but omit bind/listen steps (performed by systemd activation)                                                                                                                                                           
        TCPServer.__init__(                                                                                                                                                                                                                  
            self, server_address, handler_cls, bind_and_activate=False)                                                                                                                                                                      
        # Override Socket                                                                                                                                                                                                                    
        self.socket = socket.fromfd(                                                                                                                                                                                                         
            self.SYSTEMD_FIRST_SOCKET_FD, self.address_family, self.socket_type)                                                                                                                                                             
                                                                                                                                                                                                                                             
if __name__ == "__main__":                                                                                                                                                                                                                   
    logging.basicConfig(level=logging.INFO)
    HOST, PORT = "localhost", 9999 # not really needed here
    server = Server((HOST, PORT), Handler)
    server.serve_forever()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
