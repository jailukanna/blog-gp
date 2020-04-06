---
title: mysql example bar example (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'bar example'

Functions in program: 
* `def update_bar(value):`

Modules used in program: 
* `import utils`
* `import config_parse_lib`
* `import mysql_lib`
* `import pandas as pd`
* `import os`

## python bar example

Python mysql example: bar example

```python
#!/usr/bin/env python2.7
'''
hbar chart to display server errors stats
'''
# Standard Lib
import os
# Third Party
import pandas as pd
from bokeh.io import curdoc
from bokeh.models import ColumnDataSource, FactorRange
from bokeh.plotting import Figure
from bokeh.models.widgets import RadioButtonGroup
from bokeh.layouts import column
# Local Lib
import mysql_lib
import config_parse_lib
import utils

# based on value(int) from the RadioButtonGroup, change title and data for graphs.  I plan to include new sql calls
# here once I get it working with hardcoded values.
def update_bar(value):
    if value == 0:
        exec_plot.title.text = "WG"
        ex_data_source.data = dict(exceptions=['test_error', 'another_error_msg'],
                                   count=[41, 12],
                                   index=[0, 1])
        exec_plot.y_range = FactorRange(factors=['test_error', 'another_error_msg'])
    if value == 1:
        exec_plot.title.text = "MB"
        ex_data_source.data = dict(exceptions=['test_error2', 'another_error_msg2'],
                                   count=[31, 11],
                                   index=[0, 1])
        exec_plot.y_range = FactorRange(factors=['test_error2', 'another_error_msg2'])
    if value == 2:
        exec_plot.title.text = "ALL"
        ex_data_source.data = dict(exceptions=['java.lang.NullPointerException handler', 'kafka.producer.ProducerClosedException producer already closed'],
                                   count=[68, 77],
                                   index=[0, 1])
        exec_plot.y_range = FactorRange(factors=['java.lang.NullPointerException handler', 'kafka.producer.ProducerClosedException producer already closed'])


# where my config params are, part of employer requirements
cfg_dict = config_parse_lib.get_config_params(os.path.join(utils.app_path, "config", "master_config.ini"))
# sqlite connection to get data
sqlite_conn = mysql_lib.get_conn(cfg_dict)
# data, containing a dict of three dictionaries with the values I'd like to graph, "WG_plot", "MB_plot", "ALL_plot"
exec_data = mysql_lib.get_exec_data(sqlite_conn)

# create data frame
df = pd.DataFrame(exec_data["ALL_plot"].items(), columns=['exceptions', 'count'])
# create data source
ex_data_source = ColumnDataSource(df)
# for y axis values for later
categories = df['exceptions'].tolist()
# make plot
bar_opts = dict(y='exceptions', height=0.3)
# some things that are hardcoded would be adjusted based on the data from mysql_lib.get_exec_data().
# just trying to get it working
exec_plot = Figure(title=str("ALL"), y_range=FactorRange(factors=categories), x_range=(0, 100), height=300)
exec_plot.hbar(left=0, right='count', color='blue', source=ex_data_source, **bar_opts)

#make buttons
button_group = RadioButtonGroup(labels=["WG", "MB", "ALL"], active=2)
# add callback
button_group.on_click(update_bar)
# make basic layout, add to curdoc
layout = column(exec_plot, button_group)
curdoc().add_root(layout)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
