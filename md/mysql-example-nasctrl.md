---
title: mysql example nasctrl (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'nasctrl'

Functions in program: 
* `def get_db_subscribers():`
* `def load_config(fname):`

Modules used in program: 
* `import logging`
* `import time`
* `import re`
* `import mysql.connector`
* `import yaml`
* `import Pyro4`

## python nasctrl

Python mysql example: nasctrl

```python
#!/usr/bin/python3

import Pyro4
from ipaddress import IPv4Address, IPv4Network
import yaml
import mysql.connector
import re
import time
import logging

config = None
servers = []

POL_QUANTUM = 8000  # ipt_ratelimit rounds CIR on [CONFIG_HZ * BITS_PER_BYTE] boundary


class NAS:
    re_vlan = re.compile(r'^(b_)?sub([0-9]{1,4})\.([0-9]{1,4})$')
    re_ratelimit = re.compile(r'^(?:b_sub|sub)(\d{1,4})\.(\d{1,4}) cir (\d+)')

    def __init__(self, ipaddr, port, hmac_key, networks):
        self.ipaddr = IPv4Address(ipaddr)
        self.port = int(port)
        self.hmac = hmac_key
        self.nets = set()
        for net in networks:
            self.nets.add(IPv4Network(net))
        self.syncts = int(time.time())
        self.rpc = Pyro4.Proxy("PYRO:nasd@{}:{}".format(self.ipaddr, self.port))
        self.rpc._pyroHmacKey = self.hmac
        self.rpc._pyroTimeout = 1.5
        self.subscribers = None

    def check_ip(self, ipaddr: IPv4Address):
        for net in self.nets:
            if ipaddr in net:
                return True
        return False

    def _sync_active_subscribers(self):
        """ Get list of subscribers from NAS """
        ret = dict()
        raw_data = self.rpc.dump_addresses()
        raw_rt_data = self.rpc.dump_ratelimit()
        ratelimits = dict()
        for data in raw_rt_data:
            m = self.re_ratelimit.match(data)
            if not m:
                continue
            spvlan, cvlan, rate = m.groups()
            ratelimits[(int(spvlan), int(cvlan))] = int(rate)

        for data in raw_data:
            m = self.re_vlan.match(data[0])
            if not m:
                continue
            blocked, spvlan, cvlan = m.groups()
            spvlan = int(spvlan)
            cvlan = int(cvlan)
            ipaddr = IPv4Address(data[1])
            if (spvlan, cvlan) not in ret:
                ret[(spvlan, cvlan)] = dict(ipaddrs=set())
            ret[(spvlan, cvlan)]['ipaddrs'].add(ipaddr)

            if (spvlan, cvlan) in ratelimits:
                ret[(spvlan, cvlan)]['ratelimit'] = ratelimits[(spvlan, cvlan)]
            else:
                ret[(spvlan, cvlan)]['ratelimit'] = None

            ret[(spvlan, cvlan)]['blocked'] = 1 if blocked else 0
        self.subscribers = ret

    def sync(self, db_allsubs, discard_cache=False):
        """ Sync NAS to billing database"""

        # Filter out vlans with non-matching IPs
        db_subs = dict()
        for dbsub in db_allsubs:
            matching_ipaddrs = set()
            for ipaddr in db_allsubs[dbsub]['ipaddrs']:
                if self.check_ip(ipaddr):
                    matching_ipaddrs.add(ipaddr)
            if matching_ipaddrs:
                db_subs[dbsub] = dict(ipaddrs=matching_ipaddrs, ratelimit=db_allsubs[dbsub]['ratelimit'],
                                      blocked=db_allsubs[dbsub]['blocked'])
        # pprint(db_subs)
        if discard_cache or (self.rpc.get_syncts() != self.syncts):
            logging.info("Syncing active subscribers")
            self._sync_active_subscribers()
            self.syncts = int(time.time())
            self.rpc.update_syncts(self.syncts)

        delsubs = self.subscribers.keys() - db_subs.keys()  # Present only on NAS
        newsubs = db_subs.keys() - self.subscribers.keys()  # Present only in database
        chksubs = self.subscribers.keys() & db_subs.keys()  # Present on both

        for subscriber in delsubs:
            logging.info("Del subscriber {} {}".format(subscriber[0], subscriber[1]))
            for ipaddr in self.subscribers[subscriber]['ipaddrs']:
                logging.info('Remove IP {} vlan {} {}'.format(ipaddr, subscriber[0], subscriber[1]))
                self.rpc.del_ipaddr(str(ipaddr), subscriber[0], subscriber[1])

        for subscriber in newsubs:
            logging.info("Add subscriber {} {}".format(subscriber[0], subscriber[1]))
            for ipaddr in db_subs[subscriber]['ipaddrs']:
                logging.info('Add IP {} vlan {} {}'.format(ipaddr, subscriber[0], subscriber[1]))
                self.rpc.add_ipaddr(str(ipaddr), subscriber[0], subscriber[1])
            logging.info("Adjust bandwidth for {} {} to {} kbit".format(subscriber[0], subscriber[1],
                                                                        db_subs[subscriber]['ratelimit'] // 1024))
            self.rpc.set_ratelimit(db_subs[subscriber]['ratelimit'], subscriber[0], subscriber[1])
            logging.info("{} subscriber {} {}".format(
                'Unblock' if db_subs[subscriber]['blocked'] else "Block",
                subscriber[0], subscriber[1]
            ))
            self.rpc.set_block(db_subs[subscriber]['blocked'], subscriber[0], subscriber[1])

        for subscriber in chksubs:
            diff_field = [x for x in db_subs[subscriber].keys() if
                          self.subscribers[subscriber][x] != db_subs[subscriber][x]]
            if 'ipaddrs' in diff_field:
                ipaddr_diff = db_subs[subscriber]['ipaddrs'] - self.subscribers[subscriber]['ipaddrs']
                for ipaddr in ipaddr_diff:
                    logging.info('Add IP {} vlan {} {}'.format(ipaddr, subscriber[0], subscriber[1]))
                    self.rpc.add_ipaddr(str(ipaddr), subscriber[0], subscriber[1])
                ipaddr_diff = self.subscribers[subscriber]['ipaddrs'] - db_subs[subscriber]['ipaddrs']
                for ipaddr in ipaddr_diff:
                    logging.info('Remove IP {} vlan {} {}'.format(ipaddr, subscriber[0], subscriber[1]))
                    self.rpc.del_ipaddr(str(ipaddr), subscriber[0], subscriber[1])
            if 'ratelimit' in diff_field:
                logging.info("Adjust bandwidth for {} {} to {} kbit".format(subscriber[0], subscriber[1],
                                                                            db_subs[subscriber]['ratelimit'] // 1024))
                self.rpc.set_ratelimit(db_subs[subscriber]['ratelimit'], subscriber[0], subscriber[1])
            if 'blocked' in diff_field:
                logging.info("{} subscriber {} {}".format(
                    'Unblock' if db_subs[subscriber]['blocked'] else "Block",
                    subscriber[0], subscriber[1]
                ))
                self.rpc.set_block(db_subs[subscriber]['blocked'], subscriber[0], subscriber[1])
        self.subscribers = db_subs


def load_config(fname):
    global config
    global servers
    # re_range = re.compile(r"^([0-9]+-[0-9]+)(?:,([0-9]+-[0-9]+))$")
    config = yaml.safe_load(open(fname, 'r'))
    servers = []
    for srv in config['servers']:
        servers.append(NAS(srv['rpc_ipaddr'], srv['rpc_port'], srv['hmac_key'], srv['networks']))
    del (config['servers'])


def get_db_subscribers():
    """ Get all subscribers from db"""
    cnx = mysql.connector.connect(user=config['db_user'], password=config['db_password'],
                                  host=config['db_host'],
                                  database=config['db_schema'])
    cursor = cnx.cursor(dictionary=True, buffered=True)
    query = 'SELECT ip, spvlan, vlan as cvlan, (rx_speed * 1024 DIV {0} * {0}) as rate , (mode > 0) as blocked' \
            ' FROM {1} limit 50;'.format(POL_QUANTUM, config['db_table'])
    cursor.execute(query)

    subscribers = dict()
    for record in cursor:
        subscribers[(record['spvlan'], record['cvlan'])] = {
            'ipaddrs': set([IPv4Address(x) for x in record['ip'].split(',')]),
            'ratelimit': record['rate'],
            'blocked': record['blocked']
        }
    cnx.close()
    return subscribers


if __name__ == '__main__':
    load_config('config.yaml')
    logging.basicConfig(level=logging.DEBUG)
    # pprint(servers[0].get_active_subscribers())
    is_active = True
    lastrun = 0
    cache_expire_counter = 0
    while is_active:
        current_time = time.time()
        if current_time - lastrun > config['sync_interval']:
            lastrun = current_time
            db_subscribers = get_db_subscribers()
            for server in servers:
                try:
                    logging.debug("Run {}".format(cache_expire_counter))
                    server.sync(db_subscribers,
                                discard_cache=True if cache_expire_counter == 0 else False)
                    cache_expire_counter = (cache_expire_counter + 1) % config['cache_expire']
                except Exception as e:
                    print("Exception while syncing server {} : {}".format(server.ipaddr, e.args))
        else:
            time.sleep(1)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
