---
title: mysql example nasd (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'nasd'

Functions in program: 
* `def user_unblock(spvlan: int, cvlan: int):`
* `def user_block(spvlan: int, cvlan: int):`
* `def set_ratelimit(rate, spvlan, cvlan):`
* `def ipaddr_delete(ipaddr: str, spvlan: int, cvlan: int):`
* `def ipaddr_add(ipaddr: str, spvlan: int, cvlan: int):`
* `def create_iface(spvlan: int, cvlan: int):`
* `def get_interface_index(ifname: str):`
* `def dump_ratelimit_set(setname: str):`
* `def dump_addr_table():`
* `def parse_ifa_data(data, result):`
* `def load_config(fname):`

Modules used in program: 
* `import Pyro4`
* `import os`
* `import yaml`
* `import struct`

## python nasd

Python mysql example: nasd

```python
#!/usr/bin/python3

import struct
import yaml
import os
import Pyro4

from ipaddress import IPv4Address, IPv4Network
from pyroute2 import IPRoute
from pyroute2 import NetlinkError
from pyroute2 import IPBatch
from pyroute2 import IPRSocket
from pyroute2.netlink.rtnl import RTM_GETADDR
from pyroute2.netlink.rtnl import RTM_NEWADDR
from pyroute2.netlink import NLM_F_ROOT
from pyroute2.netlink import NLM_F_REQUEST
from pyroute2.netlink import NLMSG_DONE
from socket import AF_INET, ntohl

_config = None
ipr = IPRoute()


def load_config(fname):
    global _config
    # re_range = re.compile(r"^([0-9]+-[0-9]+)(?:,([0-9]+-[0-9]+))$")
    _config = yaml.load(open(fname, 'r'))
    for net in _config['networks']:
        net['net'] = IPv4Network(net['net'])


def parse_ifa_data(data, result):
    """
    Parse response to RTM_GETADDR and append result with (ifname, ipaddr) tuples
    On systems with thousands of interfaces manually parsing nlmsgs is 10x faster than doing it with pyroute2
    """
    NLMSG_HDRLEN = 16  # nlmsg header length
    IFADDRMSG_HDRLEN = 8  # ifaddrmsg header length
    NLA_ALIGN = 4  # All NLAs in netlink messages are aligned on 4-byte boundary
    NLA_HDRLEN = 4  # 16 bit length + 16 bit NLA type
    # NLA type
    IFA_ADDRESS = 0x1  # Remote address
    IFA_LOCAL = 0x2  # Local address
    IFA_LABEL = 0x3  # interface name

    offset = 0
    allparts = False
    while offset <= len(data) - 16:
        msglen, msgtype = struct.unpack_from('IH', data, offset=offset)

        msgborder = msglen + offset
        if msgborder > len(data):
            raise Exception("Buffer overflow")

        # It is very unlikely to have non-multpart response to RTM_GETADDR
        # So such case will not be checked

        if msgtype == NLMSG_DONE:  # All parts received. Nothing to do.
            allparts = True
            break

        if msgtype == RTM_NEWADDR:
            addr = None
            local = None
            label = None
            offset += NLMSG_HDRLEN + IFADDRMSG_HDRLEN
            while offset <= msgborder - NLA_HDRLEN:  # Should always be able to get nlmsg_header
                nlalen, nlatype = struct.unpack_from('HH', data, offset=offset)
                if nlatype == IFA_ADDRESS:
                    addr, = struct.unpack_from('I', data, offset=offset + NLA_HDRLEN)
                elif nlatype == IFA_LOCAL:
                    local, = struct.unpack_from('I', data, offset=offset + NLA_HDRLEN)
                elif nlatype == IFA_LABEL:  # last byte is NUL
                    label, = struct.unpack_from('{}s'.format(nlalen - NLA_HDRLEN - 1), data, offset=offset + NLA_HDRLEN)

                offset += (nlalen + NLA_ALIGN - 1) & ~ (NLA_ALIGN - 1)  # All NLAs are aligned on 4-byte boundary

            if (addr is not None) and (local is not None) and (addr != local) and (label is not None):
                result.append((label.decode(), ntohl(addr)))
    return allparts


def dump_addr_table():
    ipb = IPBatch()
    ipb.addr((RTM_GETADDR, NLM_F_REQUEST | NLM_F_ROOT), family=AF_INET)
    data = ipb.batch

    s = IPRSocket()
    s.sendto(data, (0, 0))
    allrecv = False
    addrs = []
    while not allrecv:
        data = s.recv(65336)
        allrecv = parse_ifa_data(data, addrs)
    return addrs


def dump_ratelimit_set(setname: str):
    """ Dump specified ratelimit set as str list"""
    if os.path.isfile("/proc/net/ipt_ratelimit/{}".format(setname)):
        f = open("/proc/net/ipt_ratelimit/{}".format(setname), 'r')
        return f.readlines()
    else:
        raise ValueError("Specified ratelimit set does not exist")


def get_interface_index(ifname: str):
    """ Get ifindex by name. Return none if specified iface doesn`t exist """
    try:
        ifindex = ipr.link("get", ifname=ifname)[0]['index']
        return ifindex
    except NetlinkError:
        return None


def create_iface(spvlan: int, cvlan: int):
    """
    Create subscriber interface. Also creates supervlan interface if needed.
    Returns vlan subif index
    """
    spvlan = int(spvlan)
    cvlan = int(cvlan)
    if not ((1 < spvlan < 4092) and (1 < cvlan < 4092)):
        raise ValueError("vlan id must be between 1 and 4092")

    id_cvlan = get_interface_index('sub{}.{}'.format(spvlan, cvlan))
    if id_cvlan is not None:
        return id_cvlan  # Interface already exists
    id_cvlan = get_interface_index('b_sub{}.{}'.format(spvlan, cvlan))
    if id_cvlan is not None:
        user_unblock(spvlan, cvlan)
        return id_cvlan  # Interface already exists

    # Check if spvlan iface exists and create it if necessary
    id_spvlan = get_interface_index('vlan{}'.format(spvlan))
    if not id_spvlan:
        for iface in _config['interfaces']:
            if iface['vlan-range'][0] <= spvlan < iface['vlan-range'][1]:
                phy = get_interface_index(iface['ifname'])
                break
        if not phy:
            raise ValueError('Wrong physical interface: {}'.format(_config['phy']))
        ipr.link("add", ifname="vlan{}".format(spvlan), kind='vlan', vlan_id=spvlan, link=phy)
        id_spvlan = get_interface_index('vlan{}'.format(spvlan))
        if not id_spvlan:
            raise Exception("Failed to create spvlan {}!".format(spvlan))

        # Backup bras would have master interface down
        # Bringing slave interface up will generate Error 100 "Network is down",
        # which can be safely ignored in this case
        try:
            ipr.link("set", index=id_spvlan, state='up')
        except NetlinkError as e:
            if e.code != 100:
                raise

    # Create cvlan interface
    ipr.link("add", ifname="sub{}.{}".format(spvlan, cvlan), kind='vlan',
             vlan_id=cvlan, link=id_spvlan)
    id_cvlan = get_interface_index('sub{}.{}'.format(spvlan, cvlan))
    if not id_cvlan:
        raise Exception("Failed to create subscriber interface {} {}".format(spvlan, cvlan))

    try:
        ipr.link("set", index=id_cvlan, state='up')
    except NetlinkError as e:  # See above for Error 100
        if e.code != 100:
            raise

    # PPS Limits for common DDoS types
    if 'pps_dns' in _config and os.path.isfile('/proc/net/ipt_ratelimit/pps_dns'):
        with open('/proc/net/ipt_ratelimit/pps_dns', 'w') as f:
            f.write("@+{} {}\n".format(id_cvlan, _config['pps_dns']))
    if 'pps_ntp' in _config and os.path.isfile('/proc/net/ipt_ratelimit/pps_ntp'):
        with open('/proc/net/ipt_ratelimit/pps_ntp', 'w') as f:
            f.write("@+{} {}\n".format(id_cvlan, _config['pps_ntp']))
    if 'pps_dhcp' in _config and os.path.isfile('/proc/net/ipt_ratelimit/pps_dhcp'):
        with open('/proc/net/ipt_ratelimit/pps_dhcp', 'w') as f:
            f.write("@+{} {}\n".format(id_cvlan, _config['pps_dhcp']))

    return id_cvlan


def ipaddr_add(ipaddr: str, spvlan: int, cvlan: int):
    """ Attach subscriber IP to interface"""

    spvlan = int(spvlan)
    cvlan = int(cvlan)
    if not ((1 < spvlan < 4092) and (1 < cvlan < 4092)):
        raise ValueError("vlan id must be between 1 and 4092")

    ipaddr = IPv4Address(ipaddr)
    ifidx = get_interface_index('sub{}.{}'.format(spvlan, cvlan))
    if not ifidx:
        ifidx = get_interface_index('b_sub{}.{}'.format(spvlan, cvlan))
        if not ifidx:
            raise ValueError("Specified interface does not exist")

    gw = None
    for net in _config['networks']:
        if ipaddr in net['net']:
            gw = net['gw']
    if not gw:
        raise ValueError("Specified IP address {} does not belong to any subscriber network".format(ipaddr))

    try:
        ipr.addr('add', address=str(ipaddr), mask=32, index=ifidx, local=gw)
    except NetlinkError as e:
        if e.code != 17:  # IP alreasy exists on interface. Can be safely ignored.
            raise


def ipaddr_delete(ipaddr: str, spvlan: int, cvlan: int):
    """ Remove subscriber IP to interface"""

    spvlan = int(spvlan)
    cvlan = int(cvlan)
    if not ((1 < spvlan < 4092) and (1 < cvlan < 4092)):
        raise ValueError("vlan id must be between 1 and 4092")

    ipaddr = IPv4Address(ipaddr)
    ifidx = get_interface_index('sub{}.{}'.format(spvlan, cvlan))
    if not ifidx:
        ifidx = get_interface_index('b_sub{}.{}'.format(spvlan, cvlan))
        if not ifidx:
            raise ValueError("Specified interface does not exist")

    gw = None
    for net in _config['networks']:
        if ipaddr in net['net']:
            gw = net['gw']
    if not gw:
        raise ValueError("Specified IP address does not belong to any subscriber network")

    ipr.addr('del', address=str(ipaddr), mask=32, index=ifidx, local=gw)


def set_ratelimit(rate, spvlan, cvlan):
    """ Limit bandwidth on user vlan"""
    spvlan = int(spvlan)
    cvlan = int(cvlan)
    if not ((1 < spvlan < 4092) and (1 < cvlan < 4092)):
        raise ValueError("vlan id must be between 1 and 4092")
    rate = int(rate)
    if rate <= 0:
        raise ValueError("Rate must me positive integer")
    ifidx = get_interface_index('sub{}.{}'.format(spvlan, cvlan))
    if not ifidx:
        ifidx = get_interface_index('b_sub{}.{}'.format(spvlan, cvlan))
        if not ifidx:
            raise ValueError("Specified interface does not exist")

    if not ('rtset_down' in _config and os.path.isfile('/proc/net/ipt_ratelimit/{}'.format(_config['rtset_down']))):
        raise FileNotFoundError("Downstream rtset does not exist")
    if not ('rtset_up' in _config and os.path.isfile('/proc/net/ipt_ratelimit/{}'.format(_config['rtset_up']))):
        raise FileNotFoundError("Upstream rtset does not exist")
    with open('/proc/net/ipt_ratelimit/{}'.format(_config['rtset_down']), 'w') as f:
        f.write("@+{} {}\n".format(ifidx, rate))
    with open('/proc/net/ipt_ratelimit/{}'.format(_config['rtset_up']), 'w') as f:
        f.write("@+{} {}\n".format(ifidx, rate))


def user_block(spvlan: int, cvlan: int):
    """ Block user """
    iface_idx = get_interface_index('sub{}.{}'.format(spvlan, cvlan))
    if iface_idx:
        ipr.link('set', index=iface_idx, state='down')
        ipr.link('set', index=iface_idx, ifname='b_sub{}.{}'.format(spvlan, cvlan))
        ipr.link('set', index=iface_idx, state='up')


def user_unblock(spvlan: int, cvlan: int):
    """ Unblock user """
    iface_idx = get_interface_index('b_sub{}.{}'.format(spvlan, cvlan))
    if iface_idx:
        ipr.link('set', index=iface_idx, state='down')
        ipr.link('set', index=iface_idx, ifname='sub{}.{}'.format(spvlan, cvlan))
        ipr.link('set', index=iface_idx, state='up')


@Pyro4.expose
class NasdRemote(object):
    def __init__(self):
        self.syncts = 0

    @staticmethod
    def test():
        return "test"

    def get_syncts(self):
        return self.syncts

    def update_syncts(self, ts):
        self.syncts = int(ts)

    @staticmethod
    def add_ipaddr(ipaddr:str, spvlan: int, cvlan: int):
        """ Add IP to subscribers's vlan"""
        create_iface(spvlan, cvlan)
        ipaddr_add(ipaddr, spvlan, cvlan)

    @staticmethod
    def del_ipaddr(ipaddr:str, spvlan: int, cvlan: int):
        """ remove IP to subscribers's vlan"""
        ipaddr_delete(ipaddr, spvlan, cvlan)

    @staticmethod
    def dump_addresses():
        return dump_addr_table()

    @staticmethod
    def dump_ratelimit():
        return dump_ratelimit_set(_config['rtset_down'])

    @staticmethod
    def set_ratelimit(rate, spvlan, cvlan):
        set_ratelimit(rate, spvlan, cvlan)

    @staticmethod
    def set_block(blocked, spvlan, cvlan):
        """ Block/unblock subscriber """
        if blocked == 0:
            user_unblock(spvlan, cvlan)
        else:
            user_block(spvlan, cvlan)


if __name__ == "__main__":
    load_config('/etc/nasd.yaml')
    daemon = Pyro4.Daemon(host=_config['rpc_ipaddr'], port=_config['rpc_port'])
    daemon._pyroHmacKey = _config['hmac_key']
    uri = daemon.register(NasdRemote, 'nasd')
    print("Managed interfaces:")
    for iface in _config['interfaces']:
            print("Iface: {} => SVLANs {}-{}".format(iface['ifname'],iface['vlan-range'][0],iface['vlan-range'][1] ))
            #if iface['vlan-range'][0] <= spvlan < iface['vlan-range'][1]:
            #    phy = get_interface_index(iface['ifname'])
            #    break
    print("Ready. Object uri =", uri)

    daemon.requestLoop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
