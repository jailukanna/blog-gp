---
title: socket example socketsTest (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socketsTest'

Functions in program: 
* `def main():`
* `def test():`
* `def getBanner(IP, PORT):`
* `def checkVulns(banner):`
* `def checkIP(three_dots):`

Modules used in program: 
* `import socket`
* `import os`
* `import sys`

## python socketsTest

Python socket example: socketsTest

```python
# Learning from `Violent Python` book
import sys
import os
import socket

# Basic variables
socket.setdefaulttimeout(1)
portList = [21,22,25,80,110]
IP_and_PORT = []

if len(sys.argv) == 2:
  filename = sys.argv[1]
  if not os.path.isfile(filename):
    print("[-] "+str(filename)+" does not exist.")
    exit(0)
  if not os.access(filename, os.R_OK):
    print("[-] Access denied.")
    exit(0)
  print("Reading Vulnerablities from file: "+filename)
else:
  filename = "vuln_banners.txt"

# Functions
def checkIP(three_dots):
  for x in range(1,255):
    for port in portList:
      #print("[+] Checking: 91.121.3."+str(x)+" : "+str(port))
      IP = three_dots+"."+str(x)
      IP_and_PORT.append((IP,port))

def checkVulns(banner):
  f = open(filename, 'r')
  for line in f.readlines():
    if line.strip("\n") in banner:
      print("[+] Server is vulnerable: "+banner.strip("\n")) 


def getBanner(IP, PORT):
  rndSocket = socket.socket()
  try:
    rndSocket.connect((IP,PORT))
    banner = rndSocket.recv(1024)
    return banner.decode("utf-8")
  except Exception as e:
    print("[-] Error "+str(IP)+":"+str(PORT)+" -> "+str(e))
    return -1

def test():
  # ftp.installgentoo.com -> /g/ftp
  # 149.126.77.236 -> thepiratesbay.cr
  DOMAIN_AND_IP_FOR_TEST = ["ftp.installgentoo.com","149.126.77.236"]

  for port in portList:
    for domain_ip in DOMAIN_AND_IP_FOR_TEST:
      print("[+] Testing "+domain_ip+":"+str(port))
      a = getBanner(domain_ip, port)
      print(a)
      #checkVulns(a)
  print("[+] Testing done.")

def main():
  test()

  checkIP("149.126.77") # Filling up IP_and_PORT list

  for ip_port in IP_and_PORT:
    current_banner = getBanner(ip_port[0],ip_port[1])
  if current_banner == -1:
    print("[>] Current banner is empty, proceeding to next one") 
  else:
    checkVulns(current_banner)

# Execution
if __name__ == '__main__':
  main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
