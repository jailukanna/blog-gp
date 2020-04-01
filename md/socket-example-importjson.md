---
title: socket example importjson (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'importjson'

Functions in program: 
* `def makehttprequest(hostname, path, port, timeout, username, password, body, method):`
* `def getrandomhost(hosts):`
* `def b64(s):`

Modules used in program: 
* `import random`
* `import base64`
* `import socket`
* `import http.client`
* `import json`

## python importjson

Python socket example: importjson

```python
import json
import http.client
import socket
import base64
import random


def b64(s):
    # because erlang.
    return base64.b64encode(s.encode('utf-8')).decode('utf-8')


def getrandomhost(hosts):
    # this should get a list of hostnames
    if isinstance(hosts, (list,)):
        numhosts = len(hosts)
        return hosts[random.randrange(0, numhosts, 1)]
    else:
        # please don't give this a string
        return hosts


def makehttprequest(hostname, path, port, timeout, username, password, body, method):
    auth = f"{username}:{password}"
    headers = {"Authorization": "Basic " + b64(auth)}
    headers["Content-Type"] = "application/json"
    try:
        conn = http.client.HTTPConnection(hostname,
                                          port,
                                          timeout)
        conn.request(method, path, body, headers)
    except ConnectionRefusedError as e:
        print(f"Connection Refused: {str(e)}")
    except socket.error as e:
        print(f"Socket Error: {str(e)}")

    try:
        response = conn.getresponse()
    except socket.timeout:
        print(f"Request Timeout: {timeout} seconds")
    except Exception as e:
        print(f"General Exception getting response: {str(e)}")

    return response.read().decode('utf-8')


# username - note if using remote hosts, this can't be guest
ruser = "guest"
# password
rpassword = "guest"
# host used for exchanges and binding creation
rhost = "localhost"
# list used to randomise queue creation location
rqhosts = ["localhost", "localhost", "localhost", "localhost"]
# api endpoint
rendpoint = "/api/definitions"
# rabbit management port
rport = 15672
# http request timeout in seconds
rtimeout = 10
# api method
rmethod = "POST"
# filename to read
filename = "rmqdata.json"

qjsonbeginning = """{"rabbit_version":"3.5.6","users":[],"parameters":[],"policies":[],"queues":["""
qjsonending = """],"exchanges":[],"bindings":[]}"""
qexchangebeginning = """{"rabbit_version":"3.5.6","users":[],"parameters":[],"policies":[],"queues":[],"exchanges":["""
qexchangeending = """],"bindings":[]}"""
qbindingbeginning = """{"rabbit_version":"3.5.6","users":[],"parameters":[],"policies":[],"queues":[],"exchanges":[],"bindings":["""
qbindingending = """]}"""

jsdata = open(filename).read()
data = json.loads(jsdata)
queues = len(data["queues"])
exchanges = len(data["exchanges"])
bindings = len(data["bindings"])
print(f"Queues Present: {queues}, Exchanges Present: {exchanges}, Bindings: {bindings}")
print("Importing Queues")
for q in data["queues"]:
    qbody = f"{qjsonbeginning}{json.dumps(q)}{qjsonending}"
    resp = makehttprequest(getrandomhost(rqhosts), rendpoint, rport, rtimeout, ruser, rpassword, qbody, rmethod)
print("Importing Exchanges")
for ex in data["exchanges"]:
    ebody = f"{qexchangebeginning}{json.dumps(ex)}{qexchangeending}"
    resp = makehttprequest(rhost, rendpoint, rport, rtimeout, ruser, rpassword, ebody, rmethod)
print("Importing Bindings")
for bi in data["bindings"]:
    bbody = f"{qbindingbeginning}{json.dumps(bi)}{qbindingending}"
    resp = makehttprequest(rhost, rendpoint, rport, rtimeout, ruser, rpassword, bbody, rmethod)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
