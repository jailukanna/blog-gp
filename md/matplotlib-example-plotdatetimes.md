---
title: matplotlib example plotdatetimes (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'plotdatetimes'

Functions in program: 
* `def plotDateTime(dtLists, labels, title): `
* `def getRandDate(i):`

Modules used in program: 
* `import matplotlib.ticker as mticker`
* `import random`
* `import datetime`
* `import matplotlib.dates as mdates`
* `import matplotlib.pyplot as plt`
* `import pandas as pd`

## python plotdatetimes

Python matplotlib example: plotdatetimes

```python
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
import datetime
import random

import matplotlib.ticker as mticker

from pandas.plotting import register_matplotlib_converters
register_matplotlib_converters()


# Generate random datetime's
def getRandDate(i):
	randDate = '{}-{}-{}'.format(2019, random.randint(1, 12), i) #random.randint(1,28))
	randTime = '{}:{}:{}'.format(random.randint(1,23), random.randint(1,59), random.randint(1,59))
	timeStrng = randDate + ' ' + randTime
	return datetime.datetime.strptime(timeStrng, '%Y-%m-%d %H:%M:%S')


def plotDateTime(dtLists, labels, title): 
	# Plotting
	fig, ax = plt.subplots()

	# Iterate over lists of datetimes and plot them. 
	count = 0 
	colors = ['ro', 'go', 'bo', 'yo']
	for dtms in dtLists:
		x_axis, y_axis = zip(*[(item.date(),item.time()) for item in dtms])
		ax.plot(x_axis, y_axis, colors[count], markersize=1, label=labels[count])
		count += 1

	# Format the axis
	ax.set_ylim(["00:00:00", "23:59:59"])
	ax.xaxis.set_major_locator(mdates.MonthLocator())
	ax.xaxis.set_major_formatter(mdates.DateFormatter('%Y-%m-%d')) 
	ax.xaxis.set_minor_locator(mdates.DayLocator())
	ax.tick_params(axis='both', which='major', labelsize=4)
	#ax.yaxis.set_major_locator(mdates.HourLocator()) 
	#ax.yaxis.set_major_formatter(mdates.DateFormatter('%H:%M'))

	# Plot everything dammit
	mticker.Locator.MAXTICKS = 2000

	# Put a legend to the right of the current axis
	fig.legend(loc=4, frameon=True, framealpha=0.1, fontsize='xx-small', bbox_to_anchor=(1, 0.5)) # loc=0

	fig.autofmt_xdate()
	plt.title(title)
	plt.ylabel('Time', fontsize=10)
	plt.xlabel('Date', fontsize=10)
	plt.show()

if __name__ == '__main__':
	dteTms1 = [getRandDate(i) for i in range(1,28)]
	dteTms2 = [getRandDate(i) for i in range(1,28)]

	dtLists = [dteTms1, dteTms2]
	labels = ['red', 'green']






```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
