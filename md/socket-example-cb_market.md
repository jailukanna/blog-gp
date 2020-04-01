---
title: socket example cb market (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'cb market'

Functions in program: 
* `def gen_uuid_this_minute():`

Modules used in program: 
* `import cbpro,uuid,json,math,time`

## python cb market

Python socket example: cb market

```python
import cbpro,uuid,json,math,time

# my personal cb pro account on staging
key='xxx'
b64secret='rrrrrrrrr'
passphrase='[pppp]'

auth_client = cbpro.AuthenticatedClient(key, b64secret, passphrase,
                                  api_url="https://api-public.sandbox.pro.coinbase.com")

def gen_uuid_this_minute():
    seq = math.floor(time.time()/60)
    return uuid.uuid3(uuid.NAMESPACE_URL, 'http://covemarkets.com/%d' % seq)

#CLOID = str(gen_uuid_this_minute())
CLOID = '7d63a40d-9e16-495b-bc5f-45bc621d98c9'
print('sending %s' % CLOID)

acct_list =  auth_client.get_accounts()

acct_map = {}

for a in acct_list:
    if a[u'trading_enabled']:
        acct_map[a[u'currency']] = a[u'id']


btc_acct = acct_map[u'BTC']

cb_side = 'sell'
order_result = auth_client.place_order('BTC-USD',
                                       cb_side,
                                       'market',
                                       size='1', 
                                       client_oid=CLOID
)

print(json.dumps(order_result, indent=4, sort_keys=True))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
