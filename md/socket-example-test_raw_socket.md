---
title: socket example test raw socket (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'test raw socket'

Functions in program: 
* `def main():`
* `def mac_to_str(mac_addr):`

Modules used in program: 
* `import struct`
* `import ipaddress`
* `import io`
* `import ctypes`
* `import time`
* `import socket`

## python test raw socket

Python socket example: test raw socket

```python
import socket
import time
import ctypes
import io
import ipaddress
import struct

ETH_P_ALL = 3
interface = "ens33"

# イーサネットタイプ
IPv4 = 0x0800
IPv6 = 0x86dd
ARP  = 0x806

# IPプロトコルタイプ
TCP =  6
UDP = 17

class EtherNetFrame(ctypes.BigEndianStructure):
    _fields_ = (
        #("preamble", ctypes.c_ubyte*7),
        #("sfd", ctypes.c_ubyte),
        ("d_host", ctypes.c_uint8*6),
        ("s_host", ctypes.c_uint8*6),
        ("type", ctypes.c_uint16),
        ("payload", ctypes.c_uint8*1500)
    )

class Ipv4Headr(ctypes.BigEndianStructure):
    _fields_ = (
        ("vit", ctypes.c_uint16),
        ("lenth", ctypes.c_uint16),
        ("id", ctypes.c_uint16),
        ("flag", ctypes.c_uint16),
        ("ttlp", ctypes.c_uint8),
        ("type", ctypes.c_uint8),
        ("chksm", ctypes.c_uint16),
        ("s_ip", ctypes.c_uint32),
        ("d_ip", ctypes.c_uint32),
        ("data", ctypes.c_uint8*1480)
    )

class UdpHeadr(ctypes.BigEndianStructure):
    _fields_ = (
        ("s_port", ctypes.c_uint16),
        ("d_port", ctypes.c_uint16),
        ("len", ctypes.c_uint16),
        ("chksm", ctypes.c_uint16),
        ("data", ctypes.c_uint8*1472)
    )
class TcpHeadr(ctypes.BigEndianStructure):
#class TcpHeadr(ctypes.Structure):
    _fields_ = (
        ("s_port", ctypes.c_uint16),
        ("d_port", ctypes.c_uint16),
        ("seq", ctypes.c_uint32),
        ("ack", ctypes.c_uint32),
        ("drc", ctypes.c_uint16),
        ("win", ctypes.c_uint16),
        ("chck", ctypes.c_uint16),
        ("ptr", ctypes.c_uint16),
        ("data", ctypes.c_uint8*1472)
    )
# バイトデータ形式のmacアドレスを文字列に変換する。
def mac_to_str(mac_addr):
    r = ""
    for i in range(6):
        val = mac_addr[i]
        val2 = '{0:02x}'.format(val)
        r = r + val2
    return r

def main():
    s=socket.socket(socket.AF_PACKET, socket.SOCK_RAW, socket.htons(ETH_P_ALL))
    mr_ifindex = socket.if_nametoindex(interface)   # c_type is int
    mr_type = 1                     # c_type is unsigned short
    mr_alen = 0                                     # c_type is unsigned short
    mr_address = b'\0'                              # c_type is unsigned char[8]
    packet_mreq = struct.pack('iHH8s',
                            mr_ifindex,
                            mr_type,
                            mr_alen,
                            mr_address)

    s.setsockopt(1, 1, packet_mreq)
    s.bind((interface, 0))
    while True:
        data = s.recv(1514)
        etf = EtherNetFrame()
        # 受信データを変換
        buffer = io.BytesIO(data)
        buffer.readinto(etf)

        # 送信先macアドレスを取得
        d_host_adr = mac_to_str(etf.d_host)
        # 送信元macアドレスを取得
        s_host_adr = mac_to_str(etf.s_host)
        # タイプを取得する
        type = etf.type
        # ペイロードを取得する。
        payload = etf.payload

        # ipv4パケットのみ処理を行う
        if type == IPv4:
            buffer = io.BytesIO(payload)
            ipv4 = Ipv4Headr()
            buffer.readinto(ipv4)
            # 送信元IPアドレスを取得する。
            src_ip = ipaddress.IPv4Address(ipv4.s_ip)
            # 送信先IPアドレスを取得する。
            dist_ip = ipaddress.IPv4Address(ipv4.d_ip)
            # ipヘッダからプロトコルタイプを取得する
            if ipv4.type == TCP:
                buffer = io.BytesIO(ipv4.data)
                tcpdata = TcpHeadr()
                buffer.readinto(tcpdata)
                print(s_host_adr, d_host_adr, src_ip, dist_ip, tcpdata.s_port, tcpdata.d_port)
    s.close()
if __name__ == "__main__":
    main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
