---
title: socket example polo ws (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'polo ws'

Functions in program: 
* `def json_decode(content):`
* `def _get(payload):`
* `def build_requests(u, d={}, h={}):`
* `def get(u):`
* `def msg(msg):`
* `def get_timestamp():`

Modules used in program: 
* `import websockets`
* `import time`
* `import datetime`
* `import threading`
* `import json`
* `import asyncio`

## python polo ws

Python socket example: polo ws

```python
#!/usr/bin/env python3
# -*- encoding : utf-8 -*-

import asyncio
import json
import threading
import datetime
import time
from requests import get as __get__
from collections import OrderedDict

import websockets

DEFAULT_TIME_OUT = 4
DEFAULT_HEADERS = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:47.0) Gecko/20100101 Firefox/47.0',
    'Content-Type': 'application/x-www-form-urlencoded'
}

GET_TICKERS_URL = 'https://poloniex.com/public?command=returnTicker'
API_LINK = 'wss://api2.poloniex.com'
SUBSCRIBE_COMMAND = '{"command":"subscribe","channel":$}'
TICKER_SUBSCRIBE = 1002
TICKER_OUTPUT = 'TICKER UPDATE {}={}(last),{}(lowestAsk),{}(highestBid),{}(percentChange),{}(baseVolume),{}(quoteVolume),{}(isFrozen),{}(high24hr),{}(low24hr)'
TRADE_OUTPUT = 'TRADE {}={}(tradeId),{}(bookSide),{}(price),{}(size),{}(timestamp)'
ORDER = {}

class PoloniexSubscriber(object):
    def __init__(self):
        tickers_data = self._get_all_tickers()
        self._tickers_list = []
        self._tickers_id = {}  # map to tranlate id (integer) to ticker name
        for ticker, data in tickers_data.items():
            self._tickers_id[data['id']] = ticker
            self._tickers_list.append(ticker)
        self._sub_thread = None
        self._event_loop = None
        self._last_seq_dic = {}

    def get_tickers(self):
        return self._tickers_list

    def _get_all_tickers(self):
        content = get(GET_TICKERS_URL)
        return content

    def start_subscribe(self):
        self._sub_thread = threading.Thread(target=self._start_subscriber)
        self._sub_thread.daemon = True
        self._sub_thread.start()

    def _start_subscriber(self):
        self._event_loop = asyncio.new_event_loop()
        asyncio.set_event_loop(self._event_loop)
        self._event_loop.run_until_complete(self._subscribe())

    async def _subscribe(self):
        async with websockets.connect(API_LINK) as websocket:
            # first subscribe to ticker channel and all tickers update channels
            await websocket.send(SUBSCRIBE_COMMAND.replace('$', str(TICKER_SUBSCRIBE)))
            for ticker in self._tickers_list:
                req = SUBSCRIBE_COMMAND.replace('$', '\"' + ticker + '\"')
                await websocket.send(req)

            # now parse received data
            while True:
                message = await websocket.recv()
                data = json.loads(message, object_pairs_hook=OrderedDict)
                if 'error' in data:
                    raise Exception('error arrived message={}'.format(message))

                if data[0] == 1010:
                    # this mean heartbeat
                    continue
                if len(data) < 2:
                    raise Exception('Short message arrived message={}'.format(message))
                if data[1] == 1:
                    # this mean the subscription is success
                    continue
                if data[1] == 0:
                    # this mean the subscription is failure
                    raise Exception('subscription failed message={}'.format(message))

                if data[0] == TICKER_SUBSCRIBE:
                    values = data[2]
                    ticker_id_int = values[0]
                    # last = values[1]
                    # lowestAsk = values[2]
                    # highestBid = values[3]
                    # percentChange = values[4]
                    # baseVolume = values[5]
                    # quoteVolume = values[6]
                    # isFrozen = values[7]
                    # high24hr = values[8]
                    # low24hr = values[9]
                    ticker_id = self._tickers_id[ticker_id_int]
                    out_list = [ticker_id] + values[1:]
                    #msg(TICKER_OUTPUT.format(*out_list))

                else:
                    ticker_id = self._tickers_id[data[0]]
                    seq = data[1]

                    for update in data[2]:
                        # this mean this is snapshot
                        if update[0] == 'i':
                            # UPDATE[1]['currencyPair'] is the ticker name
                            self._last_seq_dic[ticker_id] = seq
                            asks = []
                            for price, size in update[1]['orderBook'][0].items():
                                asks.append([price, size])

                            bids = []
                            for price, size in update[1]['orderBook'][1].items():
                                bids.append([price, size])

                            # printing just 5 levels(this book can be 3000 levels)
                            for level in asks[0:1]:
                                msg('{} book: asks((price,size)): ({},{})'.format(ticker_id, level[0], level[1]))
                            for level in bids[0:1]:
                                msg('{} book: bids((price,size)): ({},{})'.format(ticker_id, level[0], level[1]))

                        # this mean add or change or remove
                        elif update[0] == 'o':
                            if self._last_seq_dic[ticker_id] + 1 != seq:
                                raise Exception('Problem with seq number prev_seq={},message={}'.format(
                                    self._last_seq_dic[ticker_id], message))

                            price = update[2]
                            side = 'bid' if update[1] == 1 else 'ask'
                            size = update[3]
                            # this mean remove
                            if size == '0.00000000':
                                msg('Remove {},side={},price={}'.format(ticker_id, side, price))
                            # this mean add or change
                            else:
                                msg('Add or change {},side={},price={},size={}'.format(ticker_id, side, price, size))
                        # this mean trade
                        elif update[0] == 't':
                            if self._last_seq_dic[ticker_id] + 1 != seq:
                                raise Exception('Problem with seq number prev_seq={},message={}'.format(
                                    self._last_seq_dic[ticker_id], message))

                            trade_id = update[1]
                            book_side = 'bid' if update[2] == 1 else 'ask'
                            price = update[3]
                            size = update[4]
                            timestamp = update[5]
                            out_list = [ticker_id, trade_id, book_side, price, size, timestamp]
                            msg(TRADE_OUTPUT.format(*out_list))
                    self._last_seq_dic[ticker_id] = seq

def get_timestamp():
    return datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S.%f')[:-3]

def msg(msg):
    print("{} {}".format(get_timestamp(), msg))

def get(u):
    return _get(build_requests(u))

def build_requests(u, d={}, h={}):
    payload = {}
    h.update(DEFAULT_HEADERS)
    if d:
        payload['data'] = d

    payload['url'] = u
    payload['headers'] = h
    payload['timeout'] = DEFAULT_TIME_OUT
    return payload

def _get(payload):
    try:
        req = __get__(**payload)
        return json_decode(req.text)
    except Exception:
        print("[!][_get] {}".format(payload))
        return None

def json_decode(content):
    try:
        return json.loads(content)
    except Exception:
        print("[!][json_decode] {}".format(content))
        return content

if __name__ == '__main__':
    sub = PoloniexSubscriber()
    sub.start_subscribe()
    try:
        while True:
            #print("##### while True #####")
            pass
    except KeyboardInterrupt:
        # quit
        pass

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
