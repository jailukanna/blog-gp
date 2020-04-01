---
title: socket example cb acct (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'cb acct'


Modules used in program: 
* `import cbpro`

## python cb acct

Python socket example: cb acct

```python
import cbpro

# my personal cb pro account on staging
key='xxxx'
b64secret='ssss'
passphrase='ddd'


auth_client = cbpro.AuthenticatedClient(key, b64secret, passphrase,
                                  api_url="https://api-public.sandbox.pro.coinbase.com")


acct_list =  auth_client.get_accounts()

acct_map = {}

for a in acct_list:
    if a[u'trading_enabled']:
        acct_map[a[u'currency']] = a[u'id']


btc_acct = acct_map[u'BTC']

print(auth_client.buy(btc_acct))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
