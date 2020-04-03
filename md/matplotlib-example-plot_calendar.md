---
title: matplotlib example plot calendar (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plot calendar'

Functions in program: 
* `def plot_calendar_year(data, column_name, orientation='h'):`
* `def plot_calendars(data, column_name, orientation='v'):`
* `def draw_month_labels(horizontal):`
* `def draw_month_boundary(ax, df, horizontal):`
* `def draw_day_boundary(ax, num_weeks):`
* `def draw_calendar(ax, df, horizontal, column_name):`
* `def get_colors():    `
* `def set_matplotlib_params():`

Modules used in program: 
* `import calendar`
* `import matplotlib`
* `import palettable`
* `import matplotlib.colorbar as cbar`
* `import brewer2mpl`
* `import numpy as np`

## python plot calendar

Python matplotlib example: plot calendar

```python
import numpy as np
import brewer2mpl
import matplotlib.colorbar as cbar
from matplotlib import rcParams
import palettable
from dateutil import rrule
import matplotlib
import calendar
from random import random


def set_matplotlib_params():
    """
    Set matplotlib defaults to nicer values
    """
    rcParams['mathtext.default'] ='regular'
    #rcParams['axes.labelsize']   = 11
    #rcParams['xtick.labelsize']  = 11
    #rcParams['ytick.labelsize']  = 11
    #rcParams['legend.fontsize']  = 11
    rcParams['font.family']      = 'sans-serif'
    rcParams['font.serif']       = ['Helvetica']
    #rcParams['figure.figsize']   = 16, 40 ## depends on number of years


def get_colors():    
    """
    Get palettable colors, which are nicer
    """            
    bmap = palettable.colorbrewer.sequential.BuPu_9.mpl_colors
    return bmap


def draw_calendar(ax, df, horizontal, column_name):
    '''
    Draws one calendar year
    '''
    max_val = max(df[column_name])
    min_val = min(df[column_name])
    color_list = get_colors()
    color_len = len(color_list)

    for i in range(len(df.index)):
        if(df[column_name][i] > 0):
            cur_wk = dt.date(df.year[i], df.month[i], df.day[i]).isocalendar()[1]
            cur_yr = dt.date(df.year[i], df.month[i], df.day[i]).isocalendar()[0]
            if((cur_yr < df.year[i]) and (df.day[i]<7)):
                cur_wk = 0
            if(cur_yr>df.year[i]):
                cur_wk = 53
            day_of_week = df.index[i].weekday()

            #normalise each data point to val - note added a very small amount
            #to data range, so that we never get exactly 1.0
            val = float((df[column_name][i]-min_val)/float(max_val-min_val + 0.000001))
            if horizontal:
                rect = matplotlib.patches.Rectangle((cur_wk,day_of_week), 1, 1, color = color_list[int(val*color_len)])                
            else:
                rect = matplotlib.patches.Rectangle((day_of_week,cur_wk), 1, 1, color = color_list[int(val*color_len)],label='a')                

            ax.add_patch(rect)
    return


def draw_day_boundary(ax, num_weeks):
    '''
    Draws calendar grid, seperating the days and weeks in a month
    '''
    line_color = 'white'
    line_width = 0.5
    line_style = '-'

    ## lines between weeks
    for i in range(num_weeks):
        ax.plot([0, 7], [i, i],
                color=line_color, linestyle=line_style, lw=line_width)
    
    ## lines between days
    for j in range(7):
        ax.plot([j, j], [0, num_weeks],
                color=line_color, linestyle=line_style, lw=line_width)    

    return


def draw_month_boundary(ax, df, horizontal):
    line_color = 'black'
    line_width = 1.25
    line_style = '-'
    
    month_seq = rrule.rrule(rrule.MONTHLY, dtstart=df.index[0], until=df.index[-1])
    
    for mon in month_seq:
        num_days = calendar.monthrange(mon.year,mon.month)[1]
        current_week = dt.date(mon.year, mon.month, num_days).isocalendar()[1]
        current_year = dt.date(mon.year, mon.month, num_days).isocalendar()[0]
        day_of_week  = dt.date(mon.year, mon.month, num_days).weekday()

        if current_year == mon.year and mon.month <> 12:               
            if horizontal:
                ax.plot([current_week+1, current_week+1], [0, day_of_week+1],
                        color=line_color, linestyle=line_style, lw=line_width)
            else:
                ax.plot([0, day_of_week+1], [current_week+1, current_week+1],
                        color=line_color, linestyle=line_style, lw=line_width)

            if day_of_week != 6:
                if horizontal:
                    ax.plot([current_week+1, current_week], [day_of_week+1, day_of_week+1],
                            color=line_color, linestyle=line_style, lw=line_width) # Parallel to X-Axis
                    ax.plot([current_week, current_week], [day_of_week+1, 7],
                            color=line_color, linestyle=line_style, lw=line_width)
                else:
                    ax.plot([day_of_week+1, day_of_week+1], [current_week+1, current_week],
                            color=line_color, linestyle=line_style, lw=line_width) # Parallel to Y-axis
                    ax.plot([day_of_week+1, 7], [current_week, current_week],
                            color=line_color, linestyle=line_style, lw=line_width)
    return


def draw_month_labels(horizontal):
    '''
    '''
    pos_from_edge = 3
    for idx in range(1,13):
        month_name = dt.date(1900, idx, 1).strftime('%b')
        if horizontal:
            plt.text(pos_from_edge, 8, month_name, fontsize=14)
        else:
            plt.text(8, pos_from_edge, month_name, fontsize=14)
        pos_from_edge += int(random()+4.2) ## bias for 4
    return


def plot_calendars(data, column_name, orientation='v'):
    '''
    data:   already processed to have year, month and day columns, plus a numeric column,
            with data that will be shown along a colour axis
    '''
    df = data[['year','month','day', column_name]]

    ## global parameters
    horizontal = True if orientation == 'h' else False
    num_yrs = len(df['year'].unique())
    max_val = max(df[column_name])
    min_val = min(df[column_name])
    figscale = 0.4
    longueur = num_yrs*52*figscale
    largeur = num_yrs*7*figscale
    figsize = (longueur, largeur) if horizontal else (largeur, longueur)

    # Draw blank figure
    fig = plt.figure(figsize=figsize)
    set_matplotlib_params()
    plt.subplots_adjust(hspace=0.3)
    plt.axis('off')
    plt.axes().set_aspect('equal')

    for idx, year in enumerate(df['year'].unique()):
        sub_df = df[df['year'] == year]
        
        if horizontal:
            ax = plt.subplot2grid((num_yrs, 2), (idx, 1), rowspan=1, colspan=2)
        else:
            ax = plt.subplot2grid((2, num_yrs), (1, idx), rowspan=2, colspan=1)            
        ax.xaxis.tick_top()

        ax.axes.get_xaxis().set_ticks([])
        ax.axes.get_yaxis().set_ticks([])
        ax.axis('off')
        ax.set_title(str(year), fontsize=18)

        period = np.timedelta64(sub_df.index[-1] - sub_df.index[0])
        num_weeks = int(np.ceil( period/(np.timedelta64(1,'W')) )+2)

        if horizontal:
            plt.xlim(0, num_weeks)
            plt.ylim(0, 7)
        else:
            plt.xlim(0, 7)
            plt.ylim(num_weeks, 0)

        draw_calendar(ax, sub_df, horizontal, column_name)
        draw_day_boundary(ax, num_weeks)
        draw_month_boundary(ax, sub_df, horizontal)

        if year == min(df['year'].unique()) and horizontal:
            draw_month_labels(horizontal)
        elif year == max(df['year'].unique()) and not horizontal:
            draw_month_labels(horizontal)

    def draw_legend():
        # plot an overall colorbar type legend    
        ax_colorbar = plt.subplot2grid((4, num_yrs), (3,0), rowspan=1, colspan=num_yrs)   
        mappableObject = matplotlib.cm.ScalarMappable(cmap = palettable.colorbrewer.sequential.BuPu_9.mpl_colormap)
        mappableObject.set_array(np.array(df[column_name]))
        col_bar = fig.colorbar(mappableObject, cax=ax_colorbar, orientation='horizontal',
                               boundaries = np.arange(min_val, max_val, (max_val-min_val)/10))
        # You can change the boundaries kwarg to either make the scale look less boxy (increase 10)
        # or to get different values on the tick marks, or even omit it altogether to let
        col_bar.set_label(column_name)
        ax_colorbar.set_title(column_name + ' color mapping')
    
    def draw_bar_plot():
        # draw the top overall graph
        ax0 = plt.subplot2grid((4, num_yrs), (0,0), rowspan=1, colspan=num_yrs)
        x_axis = np.arange(0.325, num_yrs+1,1.1)
        bar_val = df[column_name].groupby(df['year']).mean()
        err_val = df[column_name].groupby(df['year']).std()
        ax0.bar(x_axis, bar_val, yerr=err_val,
                linewidth=0, width=0.25, color='g',
                error_kw=dict(ecolor='gray', lw=1))
        ax0.axes.get_xaxis().set_ticks([])
        ax0.spines['top'].set_visible(False)
        ax0.spines['right'].set_visible(False)
        plt.ylabel(column_name)
        ax0.axes.get_yaxis().set_ticks([])
        return
    
    plt.tight_layout()
    plt.show()
    return

plot_calendars(calendar_df, 'size', 'h')

def plot_calendar_year(data, column_name, orientation='h'):
    
    horizontal = True if orientation == 'h' else False
    figscale = 0.4
    longueur = 52*figscale
    largeur = 7*figscale
    figsize = (longueur, largeur) if horizontal else (largeur, longueur)
    
    fig = plt.figure(figsize=figsize)
    ax = fig.add_subplot(111)
    plt.axis('off')
    
    if horizontal:
        plt.xlim(0, 52)
        plt.ylim(0, 7)
    else:
        plt.xlim(0, 7)
        plt.ylim(52, 0)
    
    draw_calendar(ax, data, horizontal, column_name)
    draw_day_boundary(ax, 52)
    draw_month_boundary(ax, data, horizontal)
    
    plt.show()
    return

#plot_calendar_year(calendar_df[calendar_df.year == 2015], 'size', 'h')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
