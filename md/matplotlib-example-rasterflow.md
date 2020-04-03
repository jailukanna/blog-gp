---
title: matplotlib example rasterflow (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'rasterflow'

Functions in program: 
* `def main():`
* `def compare_sites(site_ids, start_date, end_date, attribute, names=None):`
* `def create_colormap(data, attribute, source_map):`
* `def plot_monthly_statistics(data, attribute, title):`
* `def plot_raster_hydrograph(site, start_date, end_date, attribute, title):`
* `def add_flow_gap_attributes(data):`
* `def month_formatter():`
* `def raster_plot(data, value, title, colormap=None, norm=None,`
* `def create_yearly_totals(data, attributes):`
* `def create_raster_table(data, value, ascending = True):`
* `def mark_deficit(row):`
* `def compute_gap(row):`
* `def get_targets(row):`
* `def read_data(site_id, start_date, end_date):`

Modules used in program: 
* `import usgs_data`
* `import colormap`
* `import calendar`
* `import matplotlib.cm`
* `import matplotlib.ticker as ticker`
* `import matplotlib.dates as dates`
* `import matplotlib.pyplot as plt`
* `import pandas as pd`
* `import numpy as np`

## python rasterflow

Python matplotlib example: rasterflow

```python
"""
Module to read data in the RDB format from the USGS water data
web service. An example URL call is:
http://waterservices.usgs.gov/nwis/dv/?format=rdb&indent=on&sites=06043500&startDT=1930-01-01&endDT=2014-12-31&statCd=00003

USGS URL builder:
http://waterservices.usgs.gov/rest/DV-Test-Tool.html

Choose the USGS RDB format as output.
"""
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.dates as dates
import matplotlib.ticker as ticker
from matplotlib.colors import LinearSegmentedColormap
from matplotlib.colors import LogNorm
import matplotlib.cm
import calendar
import colormap
import usgs_data

WATER_RIGHT_BOUNDARIES = [pd.Timestamp("2000-05-15").dayofyear, pd.Timestamp('2000-07-15').dayofyear]

CFS_DAY_TO_AF = 1.9835

def read_data(site_id, start_date, end_date):
    """
    Read data for the given USGS site id from start_date to
    end_date. Adds derived attributes for flow gap data.
    """
    data = usgs_data.get_flow_data(site_id, start_date, end_date)
    
    # Add new columns for easy pivoting.
    data["dayofyear"] = data.index.dayofyear
    data["year"] = data.index.year
    data["month"] = data.index.month
    
    # Append the derived attributes
    add_flow_gap_attributes(data)
    
    return data

def get_targets(row):
    """
    Create a dataset with e-flow targets given boundaries throughout the
    year.
    """
    current_day = pd.Timestamp(row['date']).dayofyear
    if WATER_RIGHT_BOUNDARIES[0] <= current_day and current_day <= WATER_RIGHT_BOUNDARIES[1]:
        return 800
    else:
        return 400

def compute_gap(row):
    """
    Calculate the difference between actual flow and the instream flow target.
    """
    return row['flow'] - row['e-flow-target']

def mark_deficit(row):
    """
    Mark days where the actual flow does not mean the instream flow value.
    """
    return 1 if row['e-flow-gap'] < 0.0 else 0

def create_raster_table(data, value, ascending = True):
    """
    Creates the raster table from the dataframe using
    year and day of year as indices and the specified
    value attribute as the value.
    """
    return data.pivot(index = 'year', columns = 'dayofyear', values = value).sort_index(ascending=ascending)

def create_yearly_totals(data, attributes):
    """
    Sum yearly totals for a given set of attribute.
    """
    sums = map(lambda a: create_raster_table(data, a).sum(axis = 1), attributes)
    result = pd.concat(sums, axis = 1)
    return result

def raster_plot(data, value, title, colormap=None, norm=None,
                show_colorbar=False):
    """
    Create a raster plot of a given attribute with day of year on the
    x-axis and year on the y-axis.
    """
    raster_table = create_raster_table(data, value, ascending = False)
    extent = [0, 365, raster_table.index.min(), raster_table.index.max()]
    min_value = data.min()[value]
    max_value = data.max()[value]
    
    plot = plt.imshow(raster_table, interpolation = 'nearest', aspect='auto',
                      extent = extent, cmap=colormap, norm=norm)
    if show_colorbar:
        colorbar = plt.colorbar()
        #colorbar.set_ticks([data.min()[value], 0, data.max()[value]])
        #colorbar.set_ticklabels([data.min()[value], 0, data.max()[value]])
    
    axes = plot.get_axes()
    axes.set_xlabel("Month")
    axes.set_ylabel("Year")
    
    months = pd.date_range("1/1/2015", periods=12, freq="M")
    half_months = months.shift(15, freq="D")
    #axes.set_xticks(months)
    major_locator = ticker.FixedLocator(months.map(lambda d: d.dayofyear))
    minor_locator = ticker.FixedLocator(half_months.map(lambda d: d.dayofyear))
    axes.xaxis.set_major_locator(major_locator)
    axes.xaxis.set_minor_locator(minor_locator)
    axes.xaxis.set_major_formatter(ticker.NullFormatter())
    minor_formatter = month_formatter()
    axes.xaxis.set_minor_formatter(minor_formatter)
    
    plt.title(title)
    
def month_formatter():
    """
    Get a matplotlib fixed formatter that will label months by their
    middle day.
    """
    months = pd.date_range("1/1/2015", periods=12, freq="M")
    half_months = months.shift(15, freq="D")
    return ticker.FixedFormatter(half_months.map(lambda d: calendar.month_abbr[d.month]))
    
def add_flow_gap_attributes(data):
    """
    Add instream flow target attributes.
    """
    data['e-flow-target'] = pd.Series(data.reset_index().apply(get_targets, axis = 1).values, index=data.index)
    data['e-flow-gap'] = data.apply(compute_gap, axis = 1)
    data['e-flow-gap-af'] = data['e-flow-gap'] * CFS_DAY_TO_AF
    data['deficit'] = data.apply(mark_deficit, axis = 1)
    
def plot_raster_hydrograph(site, start_date, end_date, attribute, title):
    """
    Plot a raster hydrograph with a given attribute.
    site: The USGS site id
    start_date: The start date for the plot
    end_date: The end date for the plot
    attribute: the attribute to plot
    title: The title of the plot.
    """
    data = read_data(site, start_date, end_date)
    
    add_flow_gap_attributes(data)
    
    colormap = create_colormap(data, attribute, matplotlib.cm.coolwarm)
    colormap.set_bad("black")
    raster_plot(data, attribute, title, colormap=colormap, show_colorbar=True)
    
def plot_monthly_statistics(data, attribute, title):
    """
    Plot the month-by-month statistics for a given attribute
    in a box plot. Will display median, IQR, +/- 1.5*IQR, and
    outliers.
    """
    plot = data.boxplot(attribute, by='month')
    plt.title(title)
    
    axes = plot.get_axes()
    
    months = pd.date_range("1/1/2015", periods=12, freq="M")
    
    axes.xaxis.set_major_formatter(ticker.FixedFormatter(months.map(lambda d: calendar.month_abbr[d.month])))
    
def create_colormap(data, attribute, source_map):
    """
    Create a colormap given a particular dataset. It
    will set the minimum value and maximum value to the dataset
    minimum and maximum and sets a zero point based on the
    data.
    """
    min_value = data.min()[attribute]
    max_value = data.max()[attribute]
    size = max_value - min_value
    zero = abs(min_value) / size
    return colormap.shiftedColorMap(source_map, midpoint = zero)

def compare_sites(site_ids, start_date, end_date, attribute, names=None):
    datasets = map(lambda site: read_data(site, start_date, end_date), site_ids)
    columns = map(lambda d: d[attribute], datasets)
    join = pd.concat(columns, axis=1)
    if names and len(names) == len(site_ids):
        join.columns = names
    else:
        join.columns = site_ids
    return join

def main():
    import sys
    if len(sys.argv) < 6:
        print("Usage: %s <site_id> <start_date> <end_date> <attribute> <title>" % sys.argv[0])
        sys.exit(-1)

    site_id = sys.argv[1]
    start_date = sys.argv[2]
    end_date = sys.argv[3]
    attribute = sys.argv[4]
    title = sys.argv[5]

    plot_raster_hydrograph(site_id, start_date, end_date, attribute, title)
    plt.show()

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
