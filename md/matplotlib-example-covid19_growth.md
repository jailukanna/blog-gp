---
title: matplotlib example covid19 growth (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'covid19 growth'

Functions in program: 
* `def main():`

Modules used in program: 
* `import numpy.linalg`
* `import requests`
* `import matplotlib.pyplot`
* `import csv`
* `import argparse`

## python covid19 growth

Python matplotlib example: covid19 growth

```python
#!/usr/bin/env python3

import argparse
import csv

import matplotlib.pyplot
import requests
import numpy.linalg

DATA_URL_BASE = 'https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/'
DATA_OPTIONS = {
        'deaths': 'time_series_covid19_deaths_global.csv',
        'infections': 'time_series_covid19_confirmed_global.csv',
        'recovered': 'time_series_covid19_recovered_global.csv'
        }

def main():
    global DATA_URL_BASE, DATA_OPTIONS

    parser = argparse.ArgumentParser(formatter_class=argparse.ArgumentDefaultsHelpFormatter)
    parser.add_argument('--country', default='US', help='Which country')
    parser.add_argument('--subdivision', default='', help='Which country subdivision')
    parser.add_argument('--days', type=int, default=20, help='How many days to look at')
    parser.add_argument('--test', type=int, default=0, help='How many recent examples to use for testing (and not train)')
    parser.add_argument('--which', choices=list(DATA_OPTIONS), default='infections', help='Which series to plot')
    parser.add_argument('--linear_y', action='store_true', default=False, help='Use linear scale for y-axis')
    parser.add_argument('--save', help='Filename of saved plot (if not specified, do not save)')
    args = parser.parse_args()

    # constants
    DATA_URL = DATA_URL_BASE + DATA_OPTIONS[args.which]

    # get the data of interest
    raw_csv = requests.get(DATA_URL).text
    reader = csv.DictReader(list(raw_csv.split('\n')))
    country_counts = None
    for row in reader:
        if row['Country/Region'].lower() == args.country.lower():
            country_counts = row
            if row['Province/State'].lower() == args.subdivision.lower():
                break
    subdivision = country_counts['Province/State']
    region_name = '{}{}'.format(country_counts['Country/Region'], ' (' + subdivision + ')' if subdivision else '')

    assert country_counts is not None, "Could not find the data you were looking for..."

    # construct the data
    recent = [v for k, v in country_counts.items()][-args.days:]
    recent = list(map(int, recent))

    train = recent[:len(recent)-args.test]
    test = recent[len(recent)-args.test:]

    safe_log = lambda x: numpy.log(x) if x else -1
    log_train = list(map(safe_log, train)) # this is our "y"
    log_test = list(map(safe_log, test))
    print('train', len(log_train), log_train)
    print('test', len(log_test), log_test)
    recent_dates = [k for k, v in country_counts.items()][-args.days:]

    # data matrix
    X = numpy.array([[1, i] for i in range(args.days - args.test)])

    # linear fit of log(y) (i.e. log(y) ~ theta[0] + theta[1] * x)
    mm = numpy.matmul
    T = numpy.transpose
    theta = mm(mm(numpy.linalg.pinv(mm(T(X), X)), T(X)), T(log_train))
    print('estimated parameters', theta)

    # compute and print(the % daily growth and predictions)
    growth = numpy.exp(theta[1])
    growth_str = '{:0.1f}'.format((growth - 1) * 100)
    print('the most recent count of {} is {}'.format(args.which, recent[-1]))
    print('the {} are growing {}% each day'.format(args.which, growth_str))
    for d in [1, 5, 10, 20]:
        prediction = int((growth ** d) * recent[-1])
        print('in {} days, at the current rate, that means {} {}'.format(d, prediction, args.which))

    # plot the data
    y = (train + test) if args.linear_y else (log_train + log_test)
    y_hat = [theta[0] + theta[1] * x for x in range(args.days)]
    if args.linear_y:
        y_hat = numpy.exp(y_hat)
    matplotlib.pyplot.plot(range(args.days), y, 'x-')
    matplotlib.pyplot.plot(range(args.days), y_hat)

    # make the labels + title + legend
    x_ticks = list(range(args.days))
    matplotlib.pyplot.xticks(x_ticks[::2], x_ticks[::-2])
    matplotlib.pyplot.xlabel('days ago')

    if not args.linear_y:
        y_min, y_max = matplotlib.pyplot.ylim()
        y_ticks = [(i * numpy.log(10), 10 ** i) for i in range(10) if y_min <= (i + 1) * numpy.log(10) and (i - 1) * numpy.log(10) <= y_max]
        matplotlib.pyplot.yticks([y[0] for y in y_ticks], [y[1] for y in y_ticks])
    matplotlib.pyplot.ylabel('number of {} reported{}'.format(args.which, '' if args.linear_y else ' (log scale)'))

    most_recent_date = list(country_counts)[-1]
    matplotlib.pyplot.title('COVID-19 {} in {}: {}% growth each day (as of {})'.format(args.which, region_name, growth_str, most_recent_date))
    matplotlib.pyplot.legend(['reported cases', 'model fit'])

    # make sure all the labels fit on the plot
    matplotlib.pyplot.tight_layout()

    if args.save:
        print('saving to', args.save)
        matplotlib.pyplot.savefig(args.save)

    matplotlib.pyplot.show()

if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
