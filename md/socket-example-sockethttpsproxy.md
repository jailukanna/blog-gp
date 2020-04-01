---
title: socket example sockethttpsproxy (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'sockethttpsproxy'

Functions in program: 
* `def dispatcher(sock, addr):`
* `def serve_forever(http_server):`

Modules used in program: 
* `import threading`
* `import ssl`
* `import socket`
* `import select`

## python sockethttpsproxy

Python socket example: sockethttpsproxy

```python
# A simple http/https socket server
# to generate your certificates : openssl req -new -x509 -keyout /etc/ssl/localcerts/certificates.pem -out /etc/ssl/localcerts/certificates.pem -days 365 -nodes
# to start the server python3 sockethttpsproxy.py
__HOST__ = 'localhost'
__BASE_PORT__ = "1080"
__FRONTEND_PORT__ = int(__BASE_PORT__)
__BACKEND_PORT_SSL__ = int(__BASE_PORT__ + "1")
__BACKEND_PORT_HTTP__ = int(__BASE_PORT__ + "2")
__CERTS_FOLDER__ = "/etc/ssl/localcerts/"
__BUFFER_SIZE__ = 4096

from http.server import BaseHTTPRequestHandler
from http.server import HTTPServer
import select
import socket
import ssl
import threading

class getHandler(BaseHTTPRequestHandler):
            
    def do_GET(self):
        self.send_response(200)
        self.end_headers()
        self.wfile.write(bytes("Get request\n","utf-8")) 
        self.wfile.write(bytes(self.headers,"utf-8")) 
        
    def do_POST(self):
        self.send_response(200)
        self.end_headers()
        self.wfile.write(bytes("Post request\n","utf-8")) 
        self.wfile.write(bytes(self.headers,"utf-8"))     

httpd_ssl = HTTPServer((__HOST__, __BACKEND_PORT_SSL__), getHandler)
httpd_ssl.socket = ssl.wrap_socket (httpd_ssl.socket, certfile=__CERTS_FOLDER__ + 'certificates.pem', server_side=True)
httpd_direct = HTTPServer((__HOST__, __BACKEND_PORT_HTTP__), getHandler)

def serve_forever(http_server):
    while True:
        http_server.serve_forever()

def dispatcher(sock, addr):
    # ssl check 
    breq = sock.recv(1)
    if breq == bytes(b'\x16'):
        port = __BACKEND_PORT_SSL__
    else:
        port = __BACKEND_PORT_HTTP__

    data = breq + sock.recv(__BUFFER_SIZE__)    
    get_sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    get_sock.connect((__HOST__, port))
    get_sock.sendall(data)
    inp = [sock, get_sock]
    try:
        while True:
            read, write, error = select.select(inp, [], [])
            for s in read:
                write_sock = inp[1] if inp[0] == s else inp[0]
                buffer = s.recv(__BUFFER_SIZE__)
                if len(buf) > 0:
                    write_sock.send(buffer)
                else :
                    raise RuntimeError("Socket content is empty !")
    except:
        pass
    
    finally : 
        sock.close()
        get_sock.close()

try:
    print('Started server ...')
    print('Listen on ' + str(__HOST__) + ':' + str(__FRONTEND_PORT__))
    threading.Thread(target=serve_forever, args=(httpd_ssl, )).start()
    threading.Thread(target=serve_forever, args=(httpd_direct, )).start()

    main_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    main_socket.bind((__HOST__, __FRONTEND_PORT__))
    main_socket.listen(10)
    while True:
        sock, on = main_socket.accept()
        threading.Thread(target=dispatcher, args=(sock, on)).start()
    
except KeyboardInterrupt:
    httpd_ssl.server_close()
    httpd_direct.server_close()
    main_socket.close()   
    print('^C received, shutting down server')


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
