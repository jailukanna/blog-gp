---
title: matplotlib example planet grader (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'planet grader'

Functions in program: 
* `def diff(planet, jdtimestamp):`
* `def date2jd(dt):`

Modules used in program: 
* `import matplotlib.dates`
* `import matplotlib.pyplot as plt`
* `import numpy as np`
* `import datetime`
* `import jdcal`
* `import de421`

## python planet grader

Python matplotlib example: planet grader

```python
import de421
from jplephem import Ephemeris
import jdcal
import datetime
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.dates

eph = Ephemeris(de421)

planets = ["sun", "mercury", "venus", "mars", "jupiter", "saturn"]
dt = datetime.datetime.now()
jdtimestamp = jdcal.gcal2jd(dt.year, dt.month, dt.day)
jdtimestamp = sum(jdtimestamp) + (dt.hour*3600 + dt.minute*60 + dt.second) / (24*3600)

def date2jd(dt):
    jdtimestamp = jdcal.gcal2jd(dt.year, dt.month, dt.day)
    return sum(jdtimestamp) + (dt.hour*3600 + dt.minute*60 + dt.second) / (24*3600)

def diff(planet, jdtimestamp):
    barycenter, barycenter_vel = eph.position_and_velocity('earthmoon', jdtimestamp)
    moon_pos, moon_vel = eph.position_and_velocity('moon', jdtimestamp)

    earth_pos = barycenter - moon_pos * eph.earth_share
    earth_vel = barycenter_vel - moon_vel * eph.earth_share
    pos, vel = eph.position_and_velocity(planet, jdtimestamp)
    # Todo: http://physics.stackexchange.com/questions/249493/mathematically-calculate-if-a-planet-is-in-retrograde

    dpos = pos - earth_pos
    dvel = vel - earth_vel

    return dpos, dvel



months = matplotlib.dates.MonthLocator()
weekdays = matplotlib.dates.WeekdayLocator()
days = matplotlib.dates.DayLocator()
fmt = matplotlib.dates.DateFormatter('%a')

ndays = 2 * 7
dates = [datetime.datetime.today() - datetime.timedelta(days=x) for x in range(ndays, -ndays, -1)]

fig, ax = plt.subplots(len(planets), sharex=True, figsize=(10, 10), dpi=100)
for idx,planet in enumerate(planets):
    distances = []
    for date in dates:
        dist, vel = diff(planet, date2jd(date))
        distances.append(np.linalg.norm(dist))
    print(distances)
    ax[idx].plot_date(dates, distances, '-')

    ax[idx].xaxis.set_major_locator(days)
    ax[idx].xaxis.set_major_formatter(fmt)
    ax[idx].autoscale_view()

    ax[idx].fmt_xdata = matplotlib.dates.DateFormatter('%Y-%m-%d')

    x1,x2,y1,y2 = ax[idx].axis()
    ax[idx].set_ylim(ymin=-max(abs(y1),abs(y2)), ymax=max(abs(y1),abs(y2)))

    ax[idx].axhline(y=0, color='k', linestyle='dotted', linewidth=1)
    ax[idx].axvline(x=datetime.datetime.today(), color='k', linestyle='dotted', linewidth=1)

    ax[idx].set_title(planet)

fig.autofmt_xdate()
fig.tight_layout()
plt.show()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
