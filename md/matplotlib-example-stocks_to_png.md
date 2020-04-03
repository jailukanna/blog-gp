---
title: matplotlib example stocks to png (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'stocks to png'


Modules used in program: 
* `import matplotlib.dates as mdates`
* `import seaborn as sns`
* `import MySQLdb`
* `import pandas`
* `import matplotlib.pyplot as plt`
* `import matplotlib`
* `import warnings`

## python stocks to png

Python matplotlib example: stocks to png

```python
#!/usr/bin/env python2

#Squelch matplotlib / seaborne error
import warnings
warnings.filterwarnings("ignore")

import matplotlib
import matplotlib.pyplot as plt
import pandas
import MySQLdb
import seaborn as sns
from matplotlib import style
import matplotlib.dates as mdates
from datetime import datetime

#Allow 'headless' graph creation
matplotlib.use('Agg')

#Self-explanatory
style.use('dark_background')

# Assuming you are using MySQL to store your stock scores and are
# simply storing the date time as dt and the stock score as x
# The script I am using:
# https://gist.github.com/discarn8/4fb8467844dd6e10eb4b0a5652b5741b

# connect to MySQL database 
conn = MySQLdb.connect(host="192.168.0.2", user="user1", passwd="user1", db="stockprices")
 
# Query the db for the last 5 days of stocks prices.  Change the 5 to whatever length you wish to graph
query = """ 
SELECT dt, x
FROM stocks
WHERE dt > DATE_ADD(CURDATE(), INTERVAL -5 DAY);
"""

#Use Pandas to read the data and assign it to a dataframe - indexing the dt column
df = pandas.read_sql(query, conn, index_col=['dt'])

#Get the last entry, of the stock prices, and assign it to the variable stocknow
stocknow=df.iloc[-1]['x'] 

#Get the current date / time
t = datetime.now()

#Designate the time / date you pulled this data and assign it to dtStr with the format "2000-12-12 12:01"
dtStr=t.strftime("%Y-%m-%d %H:%M")

fig, ax = plt.subplots()

#Put the title on your graph
plt.title('X STOCKS 5 DAY TREND ' + dtStr, fontsize=20)

#Label the Y axis
plt.ylabel('Points')

#Do a tight graph
plt.tight_layout()

#Remove this to use a light baackground
plt.style.use('dark_background')

#Put the current stock price in the middle of the graph, in big, bright, yellow
ax.annotate(stocknow, xy=(0.4,0.5), xycoords=ax.transAxes, fontsize=36, color='yellow')

#Draw a thick, green line to plot the prices
df.plot(ax=ax, color='#00fbb0', linewidth=3.3)
locs, labels = plt.xticks()

# Save the figure to file to your html directory
fig.savefig('/var/www/html/stocks.png')

# Close the figure
plt.close(fig)

#Close the db connection
conn.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
