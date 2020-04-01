---
title: socket example SendPost (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'SendPost'


Modules used in program: 
* `import socket`
* `import sys`

## python SendPost

Python socket example: SendPost

```python
import sys
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
ipaddr = socket.gethostbyname( 'devwerks.net' )
s.connect(( ipaddr,80 ))

data = "username=test&pass=blah\n\n"

header = ( """
POST /index.php HTTP/1.1
Host: devwerks.net
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.11; rv:46.0) Gecko/20100101 Firefox/46.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://google.com
Cookie: PHPSESSID=blub
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
""" )

contentLength = "Content-Length: " + str( len( data ) ) + "\n\n"
request = header + contentLength + data
s.send( request )
response = s.recv( 4096 )
s.close()
print(request)
print(response + '\n')
sys.exit( 0 )

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
