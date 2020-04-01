---
title: socket example probe dns fake response (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'probe dns fake response'


Modules used in program: 
* `import os`
* `import sys`
* `import dpkt.dns`
* `import dpkt.ip`
* `import socket`

## python probe dns fake response

Python socket example: probe dns fake response

```python
import socket
import dpkt.ip
import dpkt.dns
import sys
import os

dst = sys.argv[1]
udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM, socket.IPPROTO_UDP)
udp_socket.settimeout(2)
icmp_socket = sock = socket.socket(socket.AF_INET, socket.SOCK_RAW, socket.IPPROTO_ICMP)
icmp_socket.settimeout(2)
dns_question = dpkt.dns.DNS(qd=[dpkt.dns.DNS.Q(name='twitter.com')])

for ttl in range(1, 15):
    udp_socket.setsockopt(socket.SOL_IP, socket.IP_TTL, ttl)
    udp_socket.sendto(str(dns_question), (dst, 53))
    router_ip = '[unknown]'
    try:
        router_ip = socket.inet_ntoa(dpkt.ip.IP(icmp_socket.recv(1024)).src)
        print('via %s' % router_ip)
    except socket.error:
        print('via *')
    try:
        dns_answer = dpkt.dns.DNS(udp_socket.recv(1024))
        if dns_answer.an:
            dns_answer_for_twitter = socket.inet_ntoa(dns_answer.an[0].rdata)
        else:
            dns_answer_for_twitter = '[blank]'
        print('twitter.com resolved to %s by %s' % (dns_answer_for_twitter, router_ip))
        os._exit(0)
    except socket.error:
        pass

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
