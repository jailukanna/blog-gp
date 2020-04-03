---
title: matplotlib example relhumorme (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'relhumorme'


Modules used in program: 
* `import mpld3`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import numpy as np`
* `import pymssql`
* `import datetime`
* `import csv`

## python relhumorme

Python matplotlib example: relhumorme

```python
import csv
import datetime
from datetime import date
import pymssql
from decimal import Decimal
from pprint(import pprint)
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
import mpld3

### set some configuration parameters to test for differences
# note cenmet location:
#http://andrewsforest.oregonstate.edu/lter/about/weather/portal/CenMet/data/cenmet_225_15min_2012.html

stationaryparm = 0.1 
dbcode = "MS043"
entity = "12"
theplot = "H15MET_testplot_relhum3.png"
thehtml = "H15MET_testplot_relhum2.html"
############## DATABASE CONNECTION ########

conn = pymssql.connect(server="------------:1433",
                     user="--------",
                     password="--------",
                     database="FSDBDATA")


#     ####### EXECUTE A SQL QUERY #########

query = """SELECT DATE_TIME, RELHUM_MEAN, PROBE_CODE, RELHUM_MEAN_FLAG, RELHUM_METHOD, HEIGHT FROM FSDBDATA.dbo.MS04312
WHERE SITECODE LIKE 'H15MET' AND DATE_TIME >='1994-01-01 00:00:00' 
ORDER BY DATE_TIME ASC"""

#     # Create a database cursor
cursor = conn.cursor()
cursor.execute(query)

counter = 1
# noc stands for "no change"
noc = 'A'
thebiggestdiff = 0

acceptable_times = []
acceptable_values = []
bad_times = []
bad_values =[]

with open("H15MET_relhum_bad.csv", "w") as outfile: 

    writer = csv.writer(outfile, quoting=csv.QUOTE_NONNUMERIC)
    for row in cursor:

      # first thing I get is the datetime
      dt = row[0]
      # tt is the time tuple (see docs: )
      tt = dt.timetuple()

      # try to set the prior value of thisval to thisvalprior (if it exists)
      # set the prior value of thediff to thediffprior (if it exists)
      try:
        thisvalprior = thisval
      except NameError:
        pass
        
      # get the current value in row[1] which is the solar
      thisval = row[1]

      # if the counter is larger than 4 days, then the counter is 1
      # times by 2 because there is 2 probes
      if tt[0] >= 2012:
        if counter> 760:
          counter = 1;
          thebiggestdiff = 0.0
      else:
        if counter > 170:
          counter = 1
          thebiggestdiff = 0.0

      #if thisvalprior exists, subtract thisval from it
      try:
        thediff = abs(thisvalprior-thisval)
      except:
        thediff = thisval

      # if this difference is greater than the prior difference, then thebiggestdiff is thediff
      if thediff > thebiggestdiff:
        thebiggestdiff = thediff

      if thebiggestdiff<=stationaryparm: 
          noc = 'Q'
          writer.writerow([dbcode, entity, 'H15MET', row[4],row[5],'1p',row[2],row[0],row[1], noc, row[3]])
          try:
            bad_values.append(row[1])
            bad_times.append(dt)
          except:
            pass
          #print('hate!')    
          counter = counter + 1 
      else: 
          # noc = 'A'
          #writer.writerow([dbcode, entity, 'H15MET', row[4],row[5],'1p',row[2],row[0],row[1],noc, row[3]])
          acceptable_values.append(row[1])
          acceptable_times.append(dt)
          #print('win!')    
          counter = counter + 1 
          #print(counter)
          #print(thebiggestdiff)

# assign a figure, and create a plot with 2 axes, label them
fig, ax = plt.subplots(1)
ax.set_ylabel('relative humidity [C]')
ax.set_xlabel('date')

# plot the acceptable values in blue
dates = matplotlib.dates.date2num(acceptable_times)
matplotlib.pyplot.plot_date(dates,acceptable_values,'bo')

# plot the unacceptable values as red squares
dates2 = matplotlib.dates.date2num(bad_times)
matplotlib.pyplot.plot_date(dates2,bad_values,'rs')

fig.autofmt_xdate()

plt.savefig(theplot)

## generate HTML for a static page!
html = mpld3.fig_to_html(fig)
mpld3.save_html(fig, thehtml)


plt.close('all')
print("Done!")  

# start a webserver to look at it!
mpld3.show(fig)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
