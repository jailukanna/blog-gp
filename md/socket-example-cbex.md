---
title: socket example cbex (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'cbex'


Modules used in program: 
* `import json`
* `import time`
* `import cbpro`
* `import sys`

## python cbex

Python socket example: cbex

```python
import sys
import cbpro
import time
import json

class CBWebsocketClient(cbpro.WebsocketClient):
    
    def on_open(self):
        # self.url = "wss://ws-feed.pro.coinbase.com/"
        self.url = 'wss://ws-feed-public.sandbox.pro.coinbase.com'
        #self.products = ["BTC-USD", "ETH-USD"]
        self.products = ["BTC-USD"]
        #self.channels = ['user']
        #self.channels = ['level2','heartbeat','ticker']
        self.channels = ['full']        
        self.message_count = 0
        print("Let's count the messages!")
        self.types = {}
        
    def on_message(self, msg):
        if 'type' in msg:
            mtype = msg['type']            
            if msg['type'] in ['open', 'received', 'done', 'match']:
                print(json.dumps(msg, indent=4, sort_keys=True))

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
