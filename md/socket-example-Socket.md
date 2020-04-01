---
title: socket example Socket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'Socket'


Modules used in program: 
* `import socket `

## python Socket

Python socket example: Socket

```python
import socket 
socket.setdefaulttimeout(2)
s = socket.socket()
s.connect(("127.0.0.1", 21))
ans = s.recv(1024)
print(ans)
#processing response
if ("FreeFloat Ftp Server (Version 1.00)" in ans): 
    print("[+] FreeFloat FTP Server is vulnerable." )

elif ("3Com 3CDaemon FTP Server Version 2.0" in banner): 
    print("[+] 3CDaemon FTP Server is vulnerable." )

elif ("Ability Server 2.34" in banner): 
    print("[+] Ability FTP Server is vulnerable." )

elif ("Sami FTP Server 2.0.2" in banner): 
    print("[+] Sami FTP Server is vulnerable." )

else: 
    print("[-] FTP Server is not vulnerable." )

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
