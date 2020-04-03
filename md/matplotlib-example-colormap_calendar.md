---
title: matplotlib example colormap calendar (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'colormap calendar'

Functions in program: 
* `def draw_month_boundary(ax,df,horz):`
* `def draw_daily_lines(ax,df,horz, num_weeks):`
* `def draw_calendar(ax,df,horz):`
* `def get_colors():    `
* `def set_matplotlib_params():`

Modules used in program: 
* `import calendar`
* `import matplotlib`
* `import palettable`
* `import matplotlib.colorbar as cbar`
* `import matplotlib.pyplot as plt`
* `import pandas, pdb, os, csv, datetime, numpy, brewer2mpl`

## python colormap calendar

Python matplotlib example: colormap calendar

```python
import pandas, pdb, os, csv, datetime, numpy, brewer2mpl
import matplotlib.pyplot as plt
import matplotlib.colorbar as cbar
from matplotlib import rcParams
import palettable
from dateutil import rrule
import matplotlib
import calendar

def set_matplotlib_params():
    """
    Set matplotlib defaults to nicer values
    """            
    # rcParams dict
    rcParams['mathtext.default'] ='regular'
    rcParams['axes.labelsize']   = 11
    rcParams['xtick.labelsize']  = 11
    rcParams['ytick.labelsize']  = 11
    rcParams['legend.fontsize']  = 11
    rcParams['font.family']      = 'sans-serif'
    rcParams['font.serif']       = ['Helvetica']
    rcParams['figure.figsize']   = 7.3, 4.2
   
###############################################################################
#
#
#
#
###############################################################################
def get_colors():    
    """
    Get palettable colors, which are nicer
    """            
    bmap=palettable.colorbrewer.sequential.BuPu_9.mpl_colors
    return bmap

###############################################################################
#
#
#
#
###############################################################################
def draw_calendar(ax,df,horz):
    for i in range(len(df.index)):
        if(df[col_name][i] > 0):
            cur_wk = datetime.date(df.YEAR[i],df.MONTH[i],df.DAY[i]).isocalendar()[1]
            cur_yr = datetime.date(df.YEAR[i],df.MONTH[i],df.DAY[i]).isocalendar()[0]
            if((cur_yr < df.YEAR[i]) and (df.DAY[i]<7)):
                cur_wk = 0
            if(cur_yr>df.YEAR[i]):
                cur_wk = 53
            day_of_week = df.index[i].weekday()

            #normalise each data point to val - note added a very small amount
            #to data range, so that we never get exactly 1.0
            val = float((df[col_name][i]-min_val)/float(max_val-min_val + 0.000001))
            if(horz):
                rect = matplotlib.patches.Rectangle((cur_wk,day_of_week), 1, 1, color = color_list[int(val*color_len)])                
            else:
                rect = matplotlib.patches.Rectangle((day_of_week,cur_wk), 1, 1, color = color_list[int(val*color_len)],label='a')                

            ax.add_patch(rect)
       
###############################################################################
#
#
#
#
###############################################################################
def draw_daily_lines(ax,df,horz, num_weeks):
    clr = 'w'
    wth = 0.5
    stl = '-'
    # Draw calendar grid

    for i in range(int(num_weeks)):
        ax.plot([0,7],[i,i],color=clr,linestyle=stl,lw=wth)
    
    for j in range(7):
        ax.plot([j,j],[0,num_weeks],color=clr,linestyle=stl,lw=wth)    

    pass

###############################################################################
#
#
#
#
###############################################################################
def draw_month_boundary(ax,df,horz):
    clr = 'w'
    wth = 1.25
    stl = '-'
    month_seq = rrule.rrule(rrule.MONTHLY,dtstart=df.index[0],until=df.index[len(df.index)-1])
    for mon in month_seq:
        num_days = calendar.monthrange(mon.year,mon.month)[1]
        cur_wk = datetime.date(mon.year,mon.month,num_days).isocalendar()[1]
        cur_yr = datetime.date(mon.year,mon.month,num_days).isocalendar()[0]
        day_of_week = datetime.date(mon.year,mon.month,num_days).weekday()

        if(cur_yr == mon.year and (mon.month <> 12)):               
            if(horz):
                ax.plot([cur_wk+1,cur_wk+1],[0,day_of_week+1],color=clr,linestyle=stl,lw=wth)
            else:
                ax.plot([0,day_of_week+1],[cur_wk+1,cur_wk+1],color=clr,linestyle=stl,lw=wth)

            if (day_of_week != 6):
                if(horz):
                    ax.plot([cur_wk+1,cur_wk],[day_of_week+1,day_of_week+1],color=clr,linestyle=stl,lw=wth) # Parallel to X-Axis
                    ax.plot([cur_wk,cur_wk],[day_of_week+1,7],color=clr,linestyle=stl,lw=wth)
                else:
                    ax.plot([day_of_week+1,day_of_week+1],[cur_wk+1,cur_wk],color=clr,linestyle=stl,lw=wth) # Parallel to Y-axis
                    ax.plot([day_of_week+1,7],[cur_wk,cur_wk],color=clr,linestyle=stl,lw=wth)

if __name__ == '__main__':
    col_name       = 'NMN'   
    horz           = False

    df      = pandas.read_csv('94.DGN',skiprows=10,delim_whitespace=True,usecols=['Y','M','D','BIOM','NMN','DN'])    
    df.rename(columns={'Y':'YEAR','M':'MONTH','D':'DAY'}, inplace=True)

    df['datetime'] = df[['YEAR', 'MONTH', 'DAY']].apply(lambda s : datetime.datetime(*s),axis=1)
    df             = df.set_index('datetime')

    num_yrs = len(df['YEAR'].unique())
    max_val = max(df[col_name])
    min_val = min(df[col_name])
    color_list = get_colors()
    color_len  = len(color_list)

    # Draw blank figure
    fig = plt.figure()
    set_matplotlib_params()
    plt.subplots_adjust(hspace=0.3)
    #plt.axis('off')       
    plt.axes().set_aspect('equal')

    idx = 0
    for i in df['YEAR'].unique():        
        sub_df = df[df['YEAR']==i]

        start_date = sub_df.index[0]
        end_date   = sub_df.index[len(sub_df.index)-1]
        diff = end_date - start_date
        diff = numpy.timedelta64(diff)
        num_weeks  = numpy.ceil(diff/(numpy.timedelta64(1,'W')))+2    
        
        if(horz):
            ax  = plt.subplot2grid((num_yrs,4),(1,idx),colspan=2,rowspan=1)
        else:
            ax  = plt.subplot2grid((4,num_yrs),(1,idx),rowspan=2,colspan=1)            
        ax.xaxis.tick_top()

        ax.axes.get_xaxis().set_ticks([])
        ax.axes.get_yaxis().set_ticks([])
        ax.axis('off')
        ax.set_title(str(i),fontsize=12)

        if (horz):
            plt.xlim(0,num_weeks)
            plt.ylim(0,7)
        else:
            plt.xlim(0,7)
            plt.ylim(num_weeks,0)
        if(i == max(df['YEAR'].unique())):
            plt.text(8,3,'Jan',fontsize=10)
            plt.text(8,8,'Feb',fontsize=10)
            plt.text(8,12,'Mar',fontsize=10)
            plt.text(8,16,'Apr',fontsize=10)
            plt.text(8,21,'May',fontsize=10)
            plt.text(8,25,'Jun',fontsize=10)
            plt.text(8,29,'Jul',fontsize=10)
            plt.text(8,34,'Aug',fontsize=10)
            plt.text(8,38,'Sept',fontsize=10)
            plt.text(8,42,'Oct',fontsize=10)
            plt.text(8,47,'Nov',fontsize=10)
            plt.text(8,51,'Dec',fontsize=10)

        draw_calendar(ax,sub_df,horz)
        draw_daily_lines(ax,sub_df,horz, num_weeks)
        draw_month_boundary(ax,sub_df,horz)
        print(idx, i)
        idx += 1
        
    # plot an overall colorbar type legend    
    ax_colorbar = plt.subplot2grid((4,num_yrs), (3,0),rowspan=1,colspan=num_yrs)   
    mappableObject = matplotlib.cm.ScalarMappable(cmap = palettable.colorbrewer.sequential.BuPu_9.mpl_colormap)
    mappableObject.set_array(numpy.array(df[col_name]))
    col_bar = fig.colorbar(mappableObject, cax = ax_colorbar, orientation = 'horizontal', boundaries = numpy.arange(min_val,max_val,(max_val-min_val)/10))
    # You can change the boundaries kwarg to either make the scale look less boxy (increase 10)
    # or to get different values on the tick marks, or even omit it altogether to let
    col_bar.set_label(col_name)
    ax_colorbar.set_title(col_name + ' color mapping')
        
    #print(min_val)
    #print(max_val)
    #plt.gca().legend(loc="upper right")
   
    #draw the top overall graph
    ax0     = plt.subplot2grid((4,num_yrs), (0,0),rowspan=1,colspan=num_yrs)
    x_axis  = numpy.arange(0.325,num_yrs+1,1.1)
    bar_val = df[col_name].groupby(df['YEAR']).mean()
    err_val = df[col_name].groupby(df['YEAR']).std()
    ax0.bar(x_axis,bar_val,yerr=err_val,linewidth=0,width=0.25,color='g',
            error_kw=dict(ecolor='gray', lw=1))
    ax0.axes.get_xaxis().set_ticks([])
    ax0.spines['top'].set_visible(False)
    ax0.spines['right'].set_visible(False)
    plt.ylabel(col_name)
    ax0.axes.get_yaxis().set_ticks([])
    #plt.tight_layout()
    plt.savefig('aaa.png',dpi=900,frameon=False)
    #plt.show()
    plt.close()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
