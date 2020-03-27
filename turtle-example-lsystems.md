---
title: turtle example lsystems (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'lsystems'

Functions in program: 
* `def expand(terms, iterations):`
* `def Rule(cf):`
* `def result_to_list(result, default):`

Modules used in program: 
* `import types`

## python lsystems

Python turtle example: lsystems

```python
# Copyright Eric Cheek 2015
import types

def result_to_list(result, default):
    if isinstance(result, list):
        return result
    elif isinstance(result, tuple):
        return list(result)
    elif isinstance(result, types.NoneType):
        return [default]
    else:
        return [result]


def Rule(cf):
    symbol = cf.__name__

    def wrapper(*args):
        # TODO: support kwargs
        continuation = lambda: result_to_list(
            cf(*args), 
            wrapper(*args)
        )
        # added for pretty printing
        continuation.pretty = "%s(%s)" % (symbol, ",".join(map(str, args)))
        return continuation

    return wrapper

def expand(terms, iterations):
    if not isinstance(terms, list):
        terms = [terms]

    for i in xrange(iterations):
        newterms = []
        for t in terms:
            newterms.extend(t())
        terms = newterms

    return terms


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
