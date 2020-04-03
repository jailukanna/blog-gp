---
title: matplotlib example usgs data (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'usgs data'

Functions in program: 
* `def get_flow_data(site_id, start_date, end_date):`
* `def dateparse(s):`
* `def format_url(site, from_str, to_str):`

Modules used in program: 
* `import datetime`
* `import pandas as pd`
* `import requests`

## python usgs data

Python matplotlib example: usgs data

```python
import requests
import pandas as pd
import datetime

def format_url(site, from_str, to_str):
    baseurl = "http://waterservices.usgs.gov/nwis/dv/?format=rdb&indent=on&sites=%s&startDT=%s&endDT=%s&statCd=00003&parameterCd=00060"
    return baseurl  % (site, from_str, to_str)

def dateparse(s):
    return pd.datetime.strptime(s, "%Y-%m-%d")

def get_flow_data(site_id, start_date, end_date):
    """
    Download USGS flow data using waterservices.usgs.gov.
    site_id: The USGS gage ID
    start_date: The starting date for the data
    end_date: The end date for the data
    Returns a Pandas time series with the data.
    """
    from_str = start_date.isoformat() if isinstance(start_date, datetime.date) else start_date
    to_str = end_date.isoformat() if isinstance(end_date, datetime.date) else end_date
    url = format_url(site_id, from_str, to_str)
    data = pd.read_csv(
        url,
        comment='#',
        delimiter="\t",
        usecols=[2, 3],
        header=1,
        parse_dates=["date"],
        date_parser=dateparse,
        index_col='date',
        names=['date', 'flow'],
        na_values='Ice')
    return data
    

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
