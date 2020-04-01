---
title: socket example tcp monitor (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'tcp monitor'

Functions in program: 
* `def help():`
* `def usage():`
* `def printStats():`
* `def getStats():`
* `def cleanup():`

Modules used in program: 
* `import time`
* `import binascii`
* `import struct`
* `import os`
* `import socket`
* `import sys`

## python tcp monitor

Python socket example: tcp monitor

```python
#!/usr/bin/python
#
# Sunny Klair 2018
# Heavily modified version of http-parse-complete.c from
#
# Bertrone Matteo - Polytechnic of Turin
# November 2015
#

from __future__ import print_function
from bcc import BPF
from ctypes import *
from struct import *
from sys import argv
from socket import inet_ntop, AF_INET, AF_INET6
from datetime import datetime

import sys
import socket
import os
import struct
import binascii
import time

def cleanup():
    for key,leaf in bpf_sessions.items():
      try:
        del bpf_sessions[key]
      except:
        print("cleanup exception.")
    return

def getStats():
    stats = {}
    for key,leaf in bpf_sessions.items():
	source = inet_ntop(AF_INET, pack(">I", key.src_ip)) + ":" + str(key.src_port)
	dest = inet_ntop(AF_INET, pack(">I", key.dst_ip)) + ":" + str(key.dst_port)

	# Note: This is gross.
	# Lexicographically sort source + dest to merge sent + recv statistics
	if source < dest:
		if source in stats:
			stats[source]["tx_bytes"] = leaf.bytes
			stats[source]["tx_pkts"] = leaf.pkts
		else:
			stats[source] = {"tx_bytes": leaf.bytes, "tx_pkts": leaf.pkts, "rx_bytes": 0, "rx_pkts":0, "dest": dest}
	else:
		if dest in stats:
			stats[dest]["rx_bytes"] = leaf.bytes
			stats[dest]["rx_pkts"] = leaf.pkts
		else:
			stats[dest] = {"tx_bytes": 0, "tx_pkts": 0, "rx_bytes": leaf.bytes, "rx_pkts": leaf.pkts, "dest": source}
    return stats

# print(function)
def printStats():
    print("---------- %s ----------" % (str(datetime.now())))
    for k, v in getStats().items():
	print("[%s -> %s] [tx_bytes: %d, rx_bytes: %d] [tx_pkts: %d, rx_pkts: %d]" % (k, v["dest"], v["tx_bytes"], v["rx_bytes"], v["tx_pkts"], v["rx_pkts"]))
    return

#args
def usage():
    print("USAGE: %s [-i <if_name>]" % argv[0])
    print("")
    print("Try '%s -h' for more options." % argv[0])
    exit()

#help
def help():
    print("USAGE: %s [-i <if_name>]" % argv[0])
    print("")
    print("optional arguments:")
    print("   -h                       print(this help"))
    print("   -i if_name               select interface if_name. Default is eth0")
    print("")
    print("examples:")
    print("    tcp_monitor              # bind socket to eth0")
    print("    tcp_monitor -i wlan0     # bind socket to wlan0")
    exit()

# arguments
interface="enp0s3"

if len(argv) == 2:
  if str(argv[1]) == '-h':
    help()
  else:
    usage()

if len(argv) == 3:
  if str(argv[1]) == '-i':
    interface = argv[2]
  else:
    usage()

if len(argv) > 3:
  usage()

print(("binding socket to '%s'" % interface))

# initialize BPF - load source code from http-parse-complete.c
bpf = BPF(src_file = "tcp_monitor.c",debug = 0)

# load eBPF program tcp_monitor of type SOCKET_FILTER into the kernel eBPF vm
# more info about eBPF program types
# http://man7.org/linux/man-pages/man2/bpf.2.html
function_monitor_filter = bpf.load_func("tcp_monitor", BPF.SOCKET_FILTER)

# create raw socket, bind it to interface
# attach bpf program to socket created
BPF.attach_raw_socket(function_monitor_filter, interface)

# get pointer to bpf map of type hash
bpf_sessions = bpf.get_table("sessions")

count = 0
while 1:
  printStats()
  if count == 10:
    cleanup() # Cleanup entire map every 60 seconds
  count += 1
  time.sleep(6)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
