---
title: pygtk example firewall (snippet)
date: 2020-02-10
tags: ["python"]
---
Python pygtk example 'firewall'

Functions in program: 
* `def test():`
* `def firewall():`
* `def pkt_callback(pkt):`
* `def write_log(pkt):`
* `def warning(string):`
* `def virus_check(pkt):`
* `def set_device():`

Modules used in program: 
* `import gtk`
* `import pygtk`
* `import sys`
* `import time `
* `import pcapy`

## python firewall

Python pygtk example: firewall

```python
#!/usr/bin/python
from scapy.all import *
import pcapy
import time 
import sys

import pygtk
pygtk.require('2.0')
import gtk

syn_counter = 0
popup_open = False

def set_device():
    devs = pcapy.findalldevs()

    i = 0
    for dev in devs:
        i += 1
        print("%2d. %s" % (i, dev))

    dev_choice = input("Choose a network device to filter: ")

    if dev_choice >= len(devs) - 1 or dev_choice < 0:
        print("Error")
        exit(1)

    return devs[dev_choice - 1]


def virus_check(pkt):
    packet = str(pkt)
    VirusList = open("VirusList.txt", "r", 0)
    viruses = VirusList.read().splitlines()
    x = 0
    for virus in viruses:
        if (packet.find(virus) != -1):
            return True
    return False


def warning(string):
    global popup_open
    if not popup_open:
        popup_open = True
        message = gtk.MessageDialog(type=gtk.MESSAGE_WARNING, buttons=gtk.BUTTONS_OK)
        message.set_markup(string)
        message.run()


def write_log(pkt):
    """
    Write log of the packet
    """
    filei = open("log.txt", "w+", 0)
    filei.write(pkt.summary())
    if (virus_check(pkt)):
        filei.write(" - Stopped, suspected a virus")
        warning("Suspected virus")
    elif(syn_counter > 8):
        filei.write(" - Stopped, suspected syn flood")
        warning("Suspected SYN flood")
    else:
        filei.write("\n")
    filei.close()
    

def pkt_callback(pkt):
    """
    Analyze the packet
    """
    global syn_counter
    if TCP in pkt:
        f = pkt['TCP'].flags

        if f & 0x02: # Check if packet is SYN
            syn_counter += 1
        else:
            syn_counter = 0
            
    write_log(pkt)


def firewall():
    """
    Choose device and start the sniffing
    """
    dev = set_device()
    sniff(iface=dev, prn=pkt_callback, store=0)


def test():
    """
    Test the firewall frm pcap files instead of real time sniffing
    """
    pkts = rdpcap("3.pcap")

    for pkt in pkts:
        pkt_callback(pkt)


if len(sys.argv) == 2 and sys.argv[1] == 'test':
    test()
else:
    firewall()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
