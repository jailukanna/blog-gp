---
title: socket example cb order (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'cb order'


Modules used in program: 
* `import math`
* `import uuid`
* `import json`
* `import time`
* `import cbpro`
* `import sys`

## python cb order

Python socket example: cb order

```python
import sys
import cbpro
import time
import json
import uuid
import math

CLOID = '7d63a40d-9e16-495b-bc5f-45bc621d98c9'

class CBWebsocketClient(cbpro.WebsocketClient):

    def dump(self, msg):
        print(json.dumps(msg, indent=4, sort_keys=True))

    def gen_uuid_this_minute(self):
        seq = math.floor(time.time()/60)
        return uuid.uuid3(uuid.NAMESPACE_URL, 'http://covemarkets.com/%d' % seq)

    def on_open(self):
        self.url = 'wss://ws-feed-public.sandbox.pro.coinbase.com'
        self.products = ["BTC-USD"]
        self.channels = ['full']        
        self.message_count = 0
        self.types = {}

        self.orderid_set = set()

        self.lastoid = None
        
    def on_message(self, msg):
        if 'type' in msg:
            mtype = msg['type']
            if mtype in ['open', 'received', 'done', 'match']:
		
                if 'order_id' in msg and msg['order_id'] in self.orderid_set:
                    print
                    print('-----****----')
                    print
                    self.dump(msg)

                if 'taker_order_id' in msg and msg['taker_order_id'] in self.orderid_set:
                    print
                    print('-----**TAKER ORDER**----')
                    print
                    self.dump(msg)

                if 'maker_order_id' in msg and msg['maker_order_id'] in self.orderid_set:
                    print
                    print('-----**MAKER ORDER**----')
                    print
                    self.dump(msg)
                
                if mtype == 'received':
                    #print(msg['client_oid'])
                    #print(msg['order_id']                    )

                    #CLOID = str(gen_uuid_this_minute())

                    if CLOID != self.lastoid:
                        print("waiting for %s" % CLOID)
                        self.lastoid = CLOID                        

                    if msg['client_oid'] == CLOID:
                        self.orderid_set.add(msg['order_id'])
                        print
                        print('-----****----')
                        print
                        self.dump(msg)

		if mtype == 'done':
		    self.orderid_set.clear()
                   

        if mtype in self.types.keys():
            self.types[mtype] = self.types[mtype] + 1
        else:
            self.types[mtype] = 1
            
        self.message_count += 1
            
    def on_close(self):
        print("-- closed --")


wsClient = CBWebsocketClient()
wsClient.start()
print(wsClient.url, wsClient.products)
try:
    while True:
        print("\nMessageCount =", "%i \n" % wsClient.message_count)
        time.sleep(1)
except KeyboardInterrupt:
    wsClient.close()

for type,count in wsClient.types.iteritems():
    print('%s: %d' % (type,count))

if wsClient.error:
    sys.exit(1)
else:
    sys.exit(0)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
