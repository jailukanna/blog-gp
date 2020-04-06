---
title: mysql example working update func (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'working update func'

Functions in program: 
* `def update_bar(value):`

## python working update func

Python mysql example: working update func

```python
""" Via @bryevdv """

def update_bar(value):
    if value == 0:
        exec_plot.title.text = "WG"
        exec_plot.y_range.factors=['test_error', 'another_error_msg']
        ex_data_source.data = dict(exceptions=['test_error', 'another_error_msg'],
                                   count=[41, 12],
                                   index=[0, 1])
    if value == 1:
        exec_plot.title.text = "MB"
        exec_plot.y_range.factors=['test_error2', 'another_error_msg2']
        ex_data_source.data = dict(exceptions=['test_error2', 'another_error_msg2'],
                                   count=[31, 11],
                                   index=[0, 1])
    if value == 2:
        exec_plot.title.text = "ALL"
        exec_plot.y_range.factors=['java.lang.NullPointerException handler', 'kafka.producer.ProducerClosedException producer already closed']
        ex_data_source.data = dict(exceptions=['java.lang.NullPointerException handler', 'kafka.producer.ProducerClosedException producer already closed'],
                                   count=[68, 77],
                                   index=[0, 1])

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
