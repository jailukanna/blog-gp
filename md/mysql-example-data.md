---
title: mysql example data (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data'


## python data

Python mysql example: data

```python
# this is an example of the data I'm trying to switch between in the Fig and update via button callback / sql
exec_data = '{'ALL_plot': {'java.lang.NullPointerException handler': 68L, 'kafka.producer.ProducerClosedException producer already closed': 77L}, 'MB_plot': {'kafka.producer.ProducerClosedException producer already closed': 77L}, 'WG_plot': {'java.lang.NullPointerException handler': 68L}}'
# to df
df = pd.DataFrame(exec_data["ALL_plot"].items(), columns=['exceptions', 'count'])

# from df via df.to_dict()  I included the output from MB_plot and WG_plot too
{'exceptions': {0: 'java.lang.NullPointerException handler', 1: 'kafka.producer.ProducerClosedException producer already closed'}, 'count': {0: 68, 1: 77}}
# wg
{'exceptions': {0: 'java.lang.NullPointerException handler'}, 'count': {0: 68}}
# mb
{'exceptions': {0: 'kafka.producer.ProducerClosedException producer already closed'}, 'count': {0: 77}}



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
