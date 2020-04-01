---
title: socket example ob (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'ob'

Functions in program: 
* `def pretty_book(book):`
* `def decimal_default(obj):`

Modules used in program: 
* `import cbpro, time, json, decimal`

## python ob

Python socket example: ob

```python
import cbpro, time, json, decimal

# coinbasepro order book using cbpro websockets client

product = 'BTC-USD'

runtime = 60

def decimal_default(obj):
    if isinstance(obj, decimal.Decimal):
        return float(obj)
    raise TypeError

def pretty_book(book):

        depth_of_book=10

        if book['sequence'] >= 0:
                print("%s %d" % (product, book['sequence']))
                
                asks = book['asks']
                if len(asks) > depth_of_book:
                        for (price, qty, sha) in asks[0:depth_of_book]:
                                print("\t %02f %02f" % (price, qty))

                bids = book['bids']

                bids = sorted(bids, key=lambda b: b[0], reverse=True)
                
                if len(bids) > depth_of_book:
                        for (price, qty, sha) in bids[0:depth_of_book]:
                                print("%02f %02f" % (qty, price))
                                
                for n in range(0,2):
                        print("\n")

         
               #print(json.dumps(book, default=decimal_default, indent=4))



logfile = open('buttercup.log', 'w')               
               
order_book = cbpro.OrderBook(product_id=product,log_to=logfile)

order_book.start()

for r in range(0, runtime):
	book = order_book.get_current_book()
        pretty_book(book)
        print(order_book._sequence;)
	time.sleep(1)

order_book.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
