---
title: matplotlib example buildGantt (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'buildGantt'

Functions in program: 
* `def create_gantt_chart(taskList, outFileName):`
* `def get_task_list(AreaToChart):`
* `def _create_date(datetxt):`
* `def make_data_string(task):`
* `def get_date_string(ts):`

Modules used in program: 
* `import os`
* `import sqlite3`
* `import datetime`
* `import numpy as np`
* `import matplotlib.dates`
* `import matplotlib.font_manager as font_manager`
* `import matplotlib.pyplot as plt`
* `import datetime as dt`

## python buildGantt

Python matplotlib example: buildGantt

```python
# Tom Foutz
# 2/19/2018
# Adapted from:
# ThingsCommandline Interface Arjan van der Gaag and Alexander Willner
#   https://github.com/AlexanderWillner/things.sh
# Quick Gantt Chart with Matplotlib by sukhbinder
#   https://sukhbinder.wordpress.com/2016/05/10/quick-gantt-chart-with-matplotlib/

import datetime as dt
import matplotlib.pyplot as plt
import matplotlib.font_manager as font_manager
import matplotlib.dates
from matplotlib.dates import WEEKLY,MONTHLY, DateFormatter, rrulewrapper, RRuleLocator 
import numpy as np
import datetime
import sqlite3
import os

def get_date_string(ts):
    if ts: return datetime.datetime.utcfromtimestamp(ts).strftime('%d-%m-%Y')
    else: return None

def make_data_string(task):
    if (not task[2]) and (task[4] == 2):
        return None
    return (task[0],get_date_string(task[2]),get_date_string(task[3]),3)

def _create_date(datetxt):
    """Creates the date"""
    if datetxt:
        day,month,year=datetxt.split('-')
        date = dt.datetime(int(year), int(month), int(day))
        mdate = matplotlib.dates.date2num(date) 
        return mdate
    else:
        return None

def get_task_list(AreaToChart):
    fnameTemp = "~/Library/Containers/com.culturedcode.ThingsMac/Data/Library/Application Support/Cultured Code/Things/Things.sqlite3" 
    fname = os.path.expanduser(fnameTemp)

    fd = os.open(fname, os.O_RDONLY)
    c = sqlite3.connect('/dev/fd/%d' % fd)
    os.close(fd)    
    taskList = []
    projId = c.execute("""SELECT uuid FROM TMArea WHERE title = "%s";""" % AreaToChart).fetchall()[0][0]
    researchProjects=c.execute("""SELECT title,uuid,startDate,dueDate,start FROM TMTask WHERE trashed = 0 AND status = 0 AND type = 1 AND area = '%s' ORDER BY startdate,dueDate;""" % projId).fetchall()

    ## Primary Project SubTasks 
    for researchProject in researchProjects:
        researchProjectTitle = researchProject[0]
        taskList.append(["%s" % researchProjectTitle,None,None,1])
        researchProjectID = researchProject[1]
        subTasks = c.execute("""SELECT title,uuid,startDate,dueDate,start FROM TMTask WHERE trashed = 0 AND status = 0 AND type = 0 AND project = '%s' ORDER BY startdate,dueDate;""" % (researchProjectID)).fetchall()
        for task in subTasks: taskList.append(make_data_string(task))
        subHeadings = c.execute("""SELECT title,uuid,start FROM TMTask WHERE trashed = 0 AND status = 0 AND type = 2 AND project = '%s' ORDER BY startdate,dueDate;""" % (researchProjectID)).fetchall()
        for subHeading in subHeadings:
            subHeadingId = subHeading[1]
            taskList.append([subHeading[0],None,None,2])
            subHeadingTasks = c.execute("""SELECT title,uuid,startDate,dueDate,start FROM TMTask WHERE trashed = 0 AND status = 0 AND type = 0 AND actionGroup = '%s' ORDER BY startdate,dueDate;""" % (subHeadingId)).fetchall()
            for task in subHeadingTasks: taskList.append(make_data_string(task))
    c.close()
    return taskList

def create_gantt_chart(taskList, outFileName):
    ylabels = []; customDates = []; entryTypes = []
    for tx in taskList:
        if tx:
            ylabel,startdate,enddate,entryType=tx
            if entryType == 1:
                heading = ''
            elif entryType == 2:
                heading = ylabel + ":"
                #print(heading)
            if entryType in [1,3]:
                ylabels.append(heading + ylabel)
                entryTypes.append(entryType)
                customDates.append([_create_date(startdate),_create_date(enddate)])
    ilen=len(ylabels)
    taskDates = {}
    for i,task in enumerate(ylabels):
        taskDates[task] = customDates[i]
    fig = plt.figure(figsize=(12.8, 13.32))
    ax = fig.add_subplot(111)
    heading = ''
    barcolors = iter(plt.get_cmap('Set1').colors)
    for i in range(len(ylabels)):
        start_date,end_date = taskDates[ylabels[i]]
        if entryTypes[i] == 1:
            barecol='k'
            barcol=barcolors.next()
            fcol='k'
            fw='bold'
            fbox = None
            end_date = np.array(customDates).max()
            if not start_date:
                c = np.array(customDates,dtype=np.float) # convert to array
                start_date = c[np.isfinite(c)].min() # select lowest date that exists
            balpha = 0.8
        elif entryTypes[i] == 3:
            if start_date:
                if not end_date: # Start Date, but no End Date
                    end_date = start_date + 1
            else:
                if end_date: # Start date but not end_date, so use start_date as end_date
                    start_date = end_date - 1
                else: # No start_date or end_date, so use today
                    start_date = end_date = matplotlib.dates.date2num(dt.datetime.now()) 
            barecol=barcol
            fw='normal'
            fbox = None
            balpha = 0.4
        ax.barh((i*0.5)+0.5, end_date - start_date, left=start_date, 
            height=0.3,
            align='center', 
            edgecolor=barecol,
            color=barcol,
            alpha = balpha)
        ax.text(start_date+0.5,(i*0.5)+0.5, heading + ylabels[i], 
            #horizontalalignment='center',
            bbox=fbox,
            verticalalignment='center',
            weight=fw,color=fcol)
    # Draw Line for Today:
    nowLine = matplotlib.dates.date2num(dt.datetime.now())
    ax.plot([nowLine,nowLine],[-0.5,0.5+ilen/2.0], "--",color="red", linewidth=0.75, alpha = 0.5)
    ax.set_ylim(ymin = -0.1, ymax = ilen*0.5+0.5)
    ax.grid(color = 'gray', linestyle = ':', linewidth=0.5, alpha = 0.5)
    ax.xaxis_date()
    rule = rrulewrapper(WEEKLY, interval=1)
    #rule = rrulewrapper(MONTHLY, interval=1)
    loc = RRuleLocator(rule)
    #formatter = DateFormatter("%d-%b '%y")
    formatter = DateFormatter("%d-%b")
    ax.xaxis.set_major_locator(loc)
    ax.xaxis.set_major_formatter(formatter)
    labelsx = ax.get_xticklabels()
    plt.setp(labelsx, rotation=30, fontsize=10)
    font = font_manager.FontProperties(size='small')
    ax.legend(loc=1,prop=font)
    ax.invert_yaxis()
    fig.autofmt_xdate()
    plt.subplots_adjust(left=0.03, right=0.93, top=0.99, bottom=0.06)
    #plt.show()
    plt.savefig(os.path.expanduser(outFileName))
    
if __name__=="__main__":
    taskList = get_task_list("Research")
    create_gantt_chart(taskList,"~/Desktop/Research.png")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
