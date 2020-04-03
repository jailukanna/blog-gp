---
title: matplotlib example timesheet (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'timesheet'

Functions in program: 
* `def run(login, options):`
* `def collect_hours_daily(records, range_filter):`
* `def parse_timesheet(lines):`
* `def get_with_cache(url, timeout=None):`
* `def debug(line):`

Modules used in program: 
* `import requests`
* `import pandas as pd`
* `import numpy as np`
* `import optparse`
* `import json`
* `import math`
* `import base64`
* `import time`
* `import os`

## python timesheet

Python matplotlib example: timesheet

```python
# coding: utf-8
"""
Shows basic statistics from last month on your timesheet.
Uses median from last 60 records to try and predict how much hours you will work.
Script will not count vacations outside of Russia, so beware!
Alternatively, script can schedule for you how many hours a day you need to work from now on.

Also, script can plot some basic graphs for visualization.

TODO:
    - chart about tickets that had most hours spent

Install (on Ubuntu):
    apt-get install python-numpy python-matplotlib python-matplotlib-data python-pandas python-requests

Install (on Mac OS):
    pip install numpy matplotlib pandas requests python-dateutil==2.2

Usage: python timesheet.py [login] [--schedule] [--plot] [--money] [--month=<int>] [--year=<int>]

Options:
  -h, --help            show this help message and exit
  -m MONTH, --month=MONTH
                        month to show stats for
  -y YEAR, --year=YEAR  year to show stats for
  -p, --plot            plot charts with matplotlib
  --local               do not call external APIs
  --schedule            schedule how much you need to work instead of predict
  --no-overtime         not counting paid overtime
  --money               show predicted salary
  --test                run tests
  --debug               debug mode
"""
from __future__ import unicode_literals
from __future__ import print_function

import os
import time
import base64
import math
import json
import optparse
from itertools import groupby, ifilter, imap
from datetime import timedelta, date, datetime as dt
from collections import OrderedDict, namedtuple
from tempfile import gettempdir
import numpy as np
import pandas as pd
import requests

DEFAULT_LOGIN = 'nborisenko'
TIMESHEETS_DIR = '~/iponweb/admin/timesheet/'
ALLOWED_OVERTIME = 20  # paid monthly overtime hours
STANDARD_WORKDAY = 8  # hours in standard workday
BASE_SALARY = 0 * 1000

NOW = dt.now()  # redefine for manual testing
# NOW = dt(2015, 9, 23, 0, 5)
DAYS_FOR_STATS = 60
CACHE_EXPIRE = 2 * 24 * 60 * 60  # 2 days

parser = optparse.OptionParser(
        usage="python timesheet.py [login] [--schedule] [--plot] [--money] [--month=<int>] [--year=<int>]"
)
parser.add_option("-m", "--month", type="int",
                  default=NOW.month,
                  help="month to show stats for")
parser.add_option("-y", "--year", type="int",
                  default=NOW.year,
                  help="year to show stats for")
parser.add_option("-p", "--plot",
                  action="store_true", default=False,
                  help="plot charts with matplotlib")
parser.add_option("--local",
                  action="store_true", default=False,
                  help="do not call external APIs")
parser.add_option("--schedule",
                  action="store_true", default=False,
                  help="schedule how much you need to work from now on")
parser.add_option("--no-overtime", dest='overtime',
                  action="store_false", default=True,
                  help="not counting paid overtime")
parser.add_option("--money",
                  action="store_true", default=False,
                  help="show predicted salary")
parser.add_option("--test",
                  action="store_true", default=False,
                  help="run tests")
parser.add_option("--debug",
                  action="store_true", default=False,
                  help="debug mode")

DAY_TYPE = {
    'workfday': 0,
    'weekend': 1,
    'holiday': 2,
    'moved_workday': 3
}


def debug(line):
    if options.debug:
        newline = '\n' if line.startswith('\n') else ''
        print(newline + "DEBUG:", line.lstrip())


def get_with_cache(url, timeout=None):
    """
    First trying to get requested content from tempfile cache,
    then getting it from url
    """
    tmpdir_path = gettempdir()
    tmpfile_path = os.path.join(tmpdir_path, base64.urlsafe_b64encode(url))
    if not os.path.exists(tmpfile_path) or (time.time() - os.stat(tmpfile_path).st_ctime) > CACHE_EXPIRE:
        debug('requesting data from %s' % url)

        resp = requests.get(url, timeout=timeout)
        assert resp.status_code == 200, 'error response from "%s": %d' % (url, resp.status_code)
        data = resp.content

        with open(tmpfile_path, 'w') as tfile:
            debug('dumping data to %s' % tmpfile_path)
            tfile.write(data)
    else:
        debug('loading data from %s' % tmpfile_path)
        with open(tmpfile_path, 'r') as tfile:
            data = tfile.read()

    return data

# internal record representation for this script
Record = namedtuple('Record', ['fr', 'to', 'ticket'])


def parse_timesheet(lines):
    """
    Parse lines from timesheet to list of records with start datetime, end datetime and ticket id for each entry.
    Record example: 2015-11-03,17:43,17:54,uworkflow,RT:335951,"review"

    :type lines: list[str]
    :rtype: list[Record]

    >>> parse_timesheet([
    ...      "$version",
    ...      "2015-01-01,09:20,10:05,someq,RT:112",
    ...      "#sometag",
    ...      "2015-01-02,23:04,23:45,someq,RT:332442",
    ...      "#2015-01-02,23:04,23:45,commented,RT:332442",
    ...      "2015-01-02,23:04,23:45,someq,RT:333111",
    ...      "# EOM"
    ... ])
    [Record(fr=Timestamp('2015-01-01 09:20:00'), to=Timestamp('2015-01-01 10:05:00'), ticket=112), \
     Record(fr=Timestamp('2015-01-02 23:04:00'), to=Timestamp('2015-01-02 23:45:00'), ticket=332442), \
     Record(fr=Timestamp('2015-01-02 23:04:00'), to=Timestamp('2015-01-02 23:45:00'), ticket=333111)]
    """

    def to_format(line):
        result = None
        try:
            result = pd.Timestamp(dt.strptime(line, '%Y-%m-%d %H:%M'))
        except ValueError:
            pass
        return result

    lines = ifilter(lambda line: line.strip(), lines)
    data = imap(lambda line: line.strip().split(',')[:5], lines)

    raw_records = ifilter(lambda elem: len(elem) == 5 and not elem[0].startswith('#'), data)
    raw_records = imap(lambda (date, time1, time2, queue, t_id): (date + ' ' + time1, date + ' ' + time2, t_id),
                       raw_records)

    return filter(lambda record: record.fr and record.to,
                  map(lambda (fr, to, t_id): Record(to_format(fr), to_format(to), int(t_id[3:])), raw_records))


def collect_hours_daily(records, range_filter):
    """
    Group records filtered through {range_filter} function by start date and
    calculate worked hours per day

    :type records: list[Record]
    :type range_filter: callable
    :rtype: dict{date: float}

    >>> collect_hours_daily([
    ...     Record(dt(2015, 1, 1, 9, 20), dt(2015, 1, 1, 10, 5), 11),
    ...     Record(dt(2015, 1, 1, 12, 17),dt(2015, 1, 1, 12, 20), 2233),
    ...     Record(dt(2015, 1, 2, 23, 4), dt(2015, 1, 2, 23, 45), 3344556)
    ... ], bool)
    [(datetime.date(2015, 1, 1), 0.8), (datetime.date(2015, 1, 2), 0.68)]
    """
    records_in_range = filter(range_filter, records)

    if not records_in_range:
        raise ValueError('no records for selected range')

    date_hours = map(lambda (k, e): (k, round(sum(map(lambda rec: (rec.to - rec.fr).seconds, e)) / 3600.0, 2)),
                     (groupby(records_in_range, lambda elem: elem[0].date())))
    return date_hours


class MonthTimeframe(object):
    """
    Class to represent selected month timeframe.
    Allows to get working days, weekends, holidays for the selected month.
    Tries to get state holidays calendar from `calendar_url` if we're in Russia atm.
    """
    geo_url = 'http://ip-api.com/json'
    calendar_url = 'http://basicdata.ru/api/json/calend/'

    def __init__(self, month, year, local=False):
        """
        :type month: int
        :type year: int
        :type local: bool
        """
        self.month = month
        self.year = year
        self.local = local
        self._cached_calendar = None

    @property
    def month_bounds(self):
        """
        :rtype: tuple(date, date)

        >>> MonthTimeframe(1, 2014, local=True).month_bounds
        (datetime.date(2014, 1, 1), datetime.date(2014, 1, 31))
        >>> MonthTimeframe(9, 2015, local=True).month_bounds
        (datetime.date(2015, 9, 1), datetime.date(2015, 9, 30))
        >>> MonthTimeframe(12, 2015, local=True).month_bounds
        (datetime.date(2015, 12, 1), datetime.date(2015, 12, 31))
        """
        month_first = date(year=self.year, month=self.month, day=1)

        if self.month == 12:
            month_last = month_first.replace(day=31)
        else:
            month_last = month_first.replace(month=self.month + 1, day=1) - timedelta(days=1)

        return month_first, month_last

    @property
    def state_holidays_calendar(self):
        """
        If we are in Russia, get state holidays and moved working days for selected month.
        First check our country by user IP, then get work days and holidays for month from webservice.

        :rtype: dict{Timestamp: int}
        :return: dict{<pandas day>: <day working status>}

        >>> MonthTimeframe(month=1, year=2016, local=True).state_holidays_calendar
        {}
        """
        results = {}

        if self.local:
            debug('\nlocal mode: skipping attempt to obtain holidays info')
            return results

        try:
            debug('\jen'
                  'trying to infer country by ip via "%s"' % self.geo_url)
            country = json.loads(get_with_cache(self.geo_url, timeout=10))

            # holidays are used only in Russia
            if country["countryCode"] == "RU":
                debug('we\'re in Russia! Trying to get month holidays from "%s"' % self.calendar_url)
                calendar = json.loads(get_with_cache(self.calendar_url, timeout=10))

                month_special_days = calendar["data"][str(self.year)].get(str(self.month), {})
                results = {pd_date: month_special_days[str(pd_date.day)]["isWorking"]
                           for pd_date in pd.date_range(*self.month_bounds) if str(pd_date.day) in month_special_days}

                debug('holidays are {0}\n'.format(results) if results
                      else 'no holidays in {0}-{1}\n'.format(self.year, self.month))

        except (requests.RequestException, ValueError, AssertionError) as exc:
            # no sugar
            print("Can't get information on holidays, will try to predict without it.")
            if options.debug:
                raise exc

        return results

    @property
    def month_work_calendar(self):
        """
        Return ordered dict with days and their working status for this month.
        If we're in Russia, try to account for state holidays too.

        :rtype: OrderedDict{Timestamp: int}

        >>> MonthTimeframe(month=1, year=2016, local=True).month_work_calendar.items()[:2]
        [(Timestamp('2016-01-01 00:00:00', offset='D'), 0), (Timestamp('2016-01-02 00:00:00', offset='D'), 2)]
        """
        if self._cached_calendar is not None:
            return self._cached_calendar

        calendar = OrderedDict([
           (pd_date, DAY_TYPE['workfday'] if pd_date.weekday() < 5 else DAY_TYPE['holiday'])
           for pd_date in pd.date_range(*self.month_bounds)
        ])

        if not self.local:
            calendar.update(self.state_holidays_calendar)

        self._cached_calendar = calendar
        return self._cached_calendar

    @property
    def holidays(self):
        return [day for day, day_type in self.month_work_calendar.items() if day_type == DAY_TYPE['holiday']]

    @property
    def weekends(self):
        return [day for day, day_type in self.month_work_calendar.items() if day_type == DAY_TYPE['weekend']]

    @property
    def workdays(self):
        return [day for day, day_type in self.month_work_calendar.items()
                if day_type in {DAY_TYPE['workfday'], DAY_TYPE['moved_workday']}]

    @property
    def nonworkdays(self):
        return self.holidays + self.weekends


class WorkLog(object):
    """
    >>> timeframe = MonthTimeframe(month=1, year=2016, local=True)
    >>> log = WorkLog(timeframe)
    >>> lines = [
    ...     "some version bullshit",
    ...     "2015-12-28,11:00,12:22,sunday,RT:11", "2014-12-30,12:40,13:00,weekday,RT:11",
    ...     "2015-12-31,09:00,12:00,weekday,RT:11", "2015-01-04,12:00,16:59,sunday,RT:11",
    ...     "2016-01-03,11:00,12:22,sunday,RT:11", "2014-12-30,12:40,13:00,weekday,RT:11",
    ...     "2016-01-04,09:00,12:00,monday,RT:11", "2015-01-04,12:00,16:59,sunday,RT:11",
    ...     "2016-01-05,12:00,18:05,tuesday,RT:11", "2015-01-06,11:00,18:40,weekday,RT:11"
    ... ]
    >>> records = parse_timesheet(lines)
    >>> log.predict(records)
    >>> log.display_results(with_salary=True)
    Total 10 hours 24 minutes in 5 days
    Assumed salary is 610 units (base salary is 9,999 units)
    Details:
    	Date			Hours
    	 2016-01-03, Sunday 	 1 h. 22 m.
    	 2016-01-04, Monday 	 3 h. 0 m.
    	 2016-01-05, Tuesday 	 6 h. 4 m.
    <BLANKLINE>
    January 2016 has 168 standard working hours
    <BLANKLINE>
    >>> log.month_total_hours  # 2 holidays are on weekend so only -8
    168
    >>> log.future_weekday_hours
    4.54
    >>> log.future_weekend_hours
    0.68500000000000005
    """
    future_weekday_hours = 0
    future_weekend_hours = 0

    worked_days = OrderedDict()
    hours_worked = 0
    days_to_come = OrderedDict()
    hours_predicted = 0

    last_recorded_date = None

    mode_templates = {
        'predictions': {
            'total': "\nPredicted total {0} hours from standard {1} in {2} days",
            'salary': "Predicted salary is {0:,} units ({2}base salary is {1:,} units)",
            'details header': "Prognosis:\n\tDate\t\t\tHours",
            'chart title': "Month statistics and predictions",
            'line label': "predicted time",
            'hours hline label': "predicted\nhours"
        },
        'scheduling': {
            'total': "\nScheduled total {0} hours from standard {1} in {2} days",
            'salary': "Scheduled salary is {0:,} units ({2}base salary is {1:,} units)",
            'details header': "Schedule:\n\tDate\t\t\tHours",
            'chart title': "Month statistics and schedule",
            'line label': "scheduled time",
            'hours hline label': "scheduled\nhours"
        }
    }
    template = None

    def __init__(self, timeframe, with_overtime=True):
        """
        :type timeframe: MonthTimeframe
        :type with_overtime: bool
        """
        self.timeframe = timeframe
        self.month = timeframe.month
        self.year = timeframe.year

        self.month_first, self.month_last = timeframe.month_bounds
        self.month_range = pd.date_range(*timeframe.month_bounds)

        # collect goal with respect to the holidays
        self.month_total_hours = STANDARD_WORKDAY * len(timeframe.workdays)
        self.goal = self.month_total_hours + ALLOWED_OVERTIME if with_overtime else self.month_total_hours

    def _calculate_month_work(self, records):
        """
        Parse records from timesheet and populate internal data: medians, last month and hours worked struct
        """
        month_filter = lambda record: (record.fr.date().month, record.fr.date().year) == (self.month, self.year)
        month_hours = collect_hours_daily(records, month_filter)

        self.worked_days = OrderedDict(month_hours)
        self.hours_worked = round(sum(self.worked_days.values()), 1)

        self.last_recorded_date = month_hours[-1][0]
        debug('last_recorded_date: %s\n' % self.last_recorded_date)

    def _calculate_salary(self, hours):
        """
        Calculate approximated salary from worked hours
        with respect to amount of paid overtime

        :type hours: float
        :rtype: tuple(int, bool)
        """
        # rounding salary up depending on the base salary amount
        rounding_to = 10 ** (int(math.log10(BASE_SALARY or 1)) - 2)
        trimmed = False

        paid_hours = hours
        if hours > self.goal:
            paid_hours = self.goal
            trimmed = True

        money = int(BASE_SALARY * paid_hours / self.month_total_hours / rounding_to) * rounding_to
        return money, trimmed

    def _predict_hours(self, records):
        """
        Calculate medians (separate for weekdays and weekends) from record period determined by {DAYS_FOR_STATS}
        """
        stats_days_range = pd.date_range(self.last_recorded_date - timedelta(days=DAYS_FOR_STATS),
                                         self.last_recorded_date)
        stats_days_filter = lambda record: (record.fr.date() in stats_days_range)
        stats_days = OrderedDict(collect_hours_daily(records, stats_days_filter))

        # weekday median is calculated across last {DAYS_FOR_STATS} days
        self.future_weekday_hours = np.median(np.array([
            hours for day, hours in stats_days.items() if pd.Timestamp(day) in self.timeframe.workdays
        ]))

        # weekend median is calculated across selected month nonworking days
        self.future_weekend_hours = np.median(np.array([
            stats_days.get(pd_day.date(), 0) for pd_day in stats_days_range if pd_day in self.timeframe.nonworkdays
        ]))

    def _predict_future(self):
        """
        Fill future days with data on how many hours you will be working
        """
        for pd_date in filter(lambda pd_date: pd_date.date() >= self.predict_from, self.month_range):
            # try to fill hours with regards to weekends and holidays
            if pd_date in self.timeframe.workdays:
                day_hours = self.future_weekday_hours
            else:
                day_hours = self.future_weekend_hours

            if day_hours:
                self.hours_predicted += day_hours
                self.days_to_come[pd_date.date()] = day_hours

    @property
    def predict_from(self):
        """
        Return day from which prediction will take place
        :rtype: date
        """
        today_date = NOW.date()
        return self.last_recorded_date + timedelta(days=1) if self.last_recorded_date == today_date else today_date

    def predict(self, records):
        """
        Try to predict working hours for the rest of the month using median from last 30 days records
        """
        self._calculate_month_work(records)
        self._predict_hours(records)

        self.hours_predicted = 0

        debug('\ntrying to predict your future working hours...')
        debug('weekday median: %.1f' % self.future_weekday_hours)
        debug('weekend median: %.1f' % self.future_weekend_hours)

        self.template = self.mode_templates['predictions']

        self._predict_future()

    def schedule(self, records):
        """
        Calculate how much you need to be working every day from now on to precisely reach the goal
        """
        if (self.month, self.year) != (NOW.month, NOW.year):
            raise ValueError

        self._calculate_month_work(records)

        available_days = filter(lambda pd_date: pd_date.date() >= self.predict_from,
                                self.timeframe.workdays)

        needed_hours = self.goal - self.hours_worked
        self.future_weekend_hours = 0

        if available_days:
            self.future_weekday_hours = needed_hours / len(available_days)
        else:
            self.future_weekday_hours = 0

        debug('\nscheduling {0} hours for the next {1} work days'.format(needed_hours, len(available_days)))
        debug('sheduled hours per day: %.1f' % self.future_weekday_hours)

        self.template = self.mode_templates['scheduling']

        self._predict_future()

    def print_past(self, with_salary=False):
        print("Total %d hours %d minutes in %d days" % (
            int(self.hours_worked), int(self.hours_worked % 1 * 60), self.last_recorded_date.day
        ))
        if with_salary and not self.days_to_come:
            assumed_salary, trimmed = self._calculate_salary(self.hours_worked)
            trim_message = 'trimmed by allowed overtime, ' if trimmed else ''
            print("Assumed salary is {0:,} units ({2}base salary is {1:,} units)".format(assumed_salary,
                                                                                         BASE_SALARY,
                                                                                         trim_message))

        print("Details:\n\tDate\t\t\tHours")

        for day, hours in self.worked_days.items():
            print("\t", day.strftime("%Y-%m-%d, %A"), "\t", "%d h. %d m." % (int(hours), int(hours % 1 * 60)))

        if not self.days_to_come:
            print("\n%s has %d standard working hours\n" %
                  (self.month_first.strftime("%B %Y"), self.month_total_hours))

    def print_future(self, with_salary=False):
        if not self.days_to_come:
            debug('nothing to predict/schedule from %s' % repr(self.predict_from))
            return

        hours_in_month = int(self.hours_worked + self.hours_predicted)
        print(self.template['total'].format(hours_in_month, self.month_total_hours, self.month_last.day))

        if with_salary:
            assumed_salary, trimmed = self._calculate_salary(hours_in_month)
            trim_message = 'trimmed by allowed overtime, ' if trimmed else ''

            print(self.template['salary'].format(assumed_salary, BASE_SALARY, trim_message))

        print(self.template['details header'])
        for day, hours in self.days_to_come.items():
            print("\t", day.strftime("%Y-%m-%d, %A"), "\t", "%d h. %d m." % (int(hours), int(hours % 1 * 60)))

    def plot_charts(self, with_salary=False):
        """
        Plot 2 charts:
            1. line chart with worked time this month + prediction / scheduled
            2. bar chart with worked hour per day data

        :type with_salary: bool
        :param with_salary: flag if month statistics chart should show assumed salary instead of hours
        """
        import matplotlib
        import matplotlib.pyplot as plt

        # check if we're working under old matplotlib from Ubuntu repo
        _old_matplotlib = (float(matplotlib.__version__.rsplit('.', 1)[0]) < 1.4)

        if _old_matplotlib:
            pd.options.display.mpl_style = 'default'
        else:
            plt.style.use('ggplot')

        self.plot_month_statistics_chart(plt, _old_matplotlib, with_salary)
        self.plot_hours_per_day(plt, _old_matplotlib)

        # show both charts
        if _old_matplotlib:
            plt.show(block=True)
        else:
            plt.show()

    def plot_month_statistics_chart(self, plt, old_matplotlib=False, with_salary=False):
        """
        Month statictics chart

        :type plt: 'pyplot.py'
        :param plt: matplotlib.pyplot module - it's stateful so better to send it in
                    every function where it's needed explicitly
        :type old_matplotlib: bool
        :param old_matplotlib: flag if code is working under matplotlib < 1.4
        :type with_salary: bool
        :param with_salary: flag if chart should show assumed salary instead of hours
        """
        acc_worked = OrderedDict()
        acc_predicted = OrderedDict()
        worked_accumulator = 0
        predicted_accumulator = 0

        debug('\nplotting prediction from %s' % repr(self.predict_from))

        # collecting data for chart
        for pd_date in self.month_range:

            day_worked = self.worked_days.get(pd_date.date(), 0)
            if pd_date.date() < self.predict_from:
                worked_accumulator += day_worked
                acc_worked[pd_date.day] = round(worked_accumulator, 2)

            if pd_date.date() == self.predict_from:
                # we need to start "predicted" plot from the last point of "worked" plot
                predicted_accumulator = worked_accumulator
                acc_predicted[acc_worked.keys()[-1]] = round(predicted_accumulator, 2)

            if pd_date.date() >= self.predict_from:
                predicted_accumulator += self.days_to_come.get(pd_date.date(), 0)
                acc_predicted[pd_date.day] = round(predicted_accumulator, 2)

        plt.figure(self.template['chart title'], figsize=(12, 6))
        line1, = plt.plot(acc_worked.keys(), acc_worked.values(), color="cornflowerblue", label="worked time")
        line2, = plt.plot(acc_predicted.keys(), acc_predicted.values(),
                          color="indianred", linestyle='--', label=self.template['line label'])

        if not old_matplotlib:
            plt.legend(handles=[line1, line2], loc=2)

        plt.xticks(np.arange(1, self.month_last.day, 1))

        plt.title(self.last_recorded_date.strftime("%B, %Y"))

        max_hours = acc_predicted.values()[-1] if acc_predicted else acc_worked.values()[-1]
        paid_overtime = self.month_total_hours + ALLOWED_OVERTIME

        plt.axis([1, self.month_last.day, 0, max(paid_overtime, max_hours) + 10])
        plt.ylabel('hours')
        plt.xlabel('day of the month')

        if self.month == NOW.month:
            plt.axvline(x=NOW.day, color='darkseagreen')

        plt.axhline(y=self.month_total_hours, color='olivedrab', linestyle='--')
        plt.text(x=14, y=self.month_total_hours + 2, s="standard work hours", color='olivedrab')
        plt.axhline(y=paid_overtime, color='mediumpurple', linestyle='--')
        plt.text(x=14.7, y=paid_overtime + 2, s="paid overtime", color='mediumpurple')

        # side labels on month statictic chart
        side_text_predicted = '  {0} hours'.format(int(max_hours))
        side_text_overtime = '  {0} hours'.format(paid_overtime)
        trimmed = False
        if with_salary:
            predicted_salary, trimmed = self._calculate_salary(max_hours)

            side_text_predicted = '  {0:,} units'.format(predicted_salary)
            # base pay + all possible pay for overtime, not rounded - we can be sure it's a right amount
            side_text_overtime = '  {0:,} units'.format(int(BASE_SALARY * (paid_overtime) / self.month_total_hours))

        if not (with_salary and trimmed):
            # no point to show more money than we can have
            plt.text(x=self.month_last.day, y=max_hours - 1, s=side_text_predicted,
                     color="indianred" if acc_predicted else "cornflowerblue", fontdict={'size': 12})

        if abs(paid_overtime - max_hours) > 5 or (with_salary and trimmed):
            # to not place one label atop of another
            plt.text(x=self.month_last.day, y=paid_overtime - 1, s=side_text_overtime,
                     color="mediumpurple", fontdict={'size': 12})

    def plot_hours_per_day(self, plt, old_matplotlib=False):
        """
        Hours worked per day chart

        :type plt: 'pyplot.py'
        :param plt: pyplot module - it's stateful so better to send it in every function where it's needed explicitly
        :type old_matplotlib: bool
        :param old_matplotlib: flag if code is working under matplotlib < 1.4
        """
        padded_worked = OrderedDict()

        # collecting data for chart
        for pd_date in self.month_range:
            day_worked = self.worked_days.get(pd_date.date(), 0)
            padded_worked[pd_date.day] = day_worked

        df2 = pd.DataFrame({'Worked': padded_worked.values()}, index=[day.day for day in self.month_range])
        df2.plot(legend=False, figsize=(10, 4), kind='bar',
                 title=self.last_recorded_date.strftime("%B, %Y"), color="cornflowerblue")
        plt.gcf().canvas.set_window_title('Hours worked daily')
        plt.ylabel('hours')
        plt.xlabel('day of the month')

        plt.axhline(y=self.future_weekday_hours, color="indianred", linestyle="--")

        if self.month == NOW.month:
            plt.axvline(x=NOW.day - 1, color='darkseagreen')

        plt.text(x=self.month_last.day, y=self.future_weekday_hours - 0.5,
                 s=self.template['hours hline label'], color="indianred", fontdict={'size': 12})

    def display_results(self, plot=False, with_salary=False):
        self.print_past(with_salary)
        self.print_future(with_salary)

        if plot:
            self.plot_charts(with_salary)


def run(login, options):
    timesheets_path = os.path.abspath(os.path.expanduser(TIMESHEETS_DIR))
    with open(os.path.join(timesheets_path, login)) as datafile:
        lines = datafile.readlines()

    timeframe = MonthTimeframe(options.month, year=options.year, local=options.local)

    records = parse_timesheet(lines)
    worklog = WorkLog(timeframe=timeframe, with_overtime=options.overtime)

    try:
        if options.schedule:
            worklog.schedule(records)
        else:
            worklog.predict(records)

        worklog.display_results(plot=options.plot, with_salary=options.money)

    except ValueError as exc:
        print("sorry, nothing to show for selected month")
        debug(repr(exc))
        return


if __name__ == '__main__':
    (options, args) = parser.parse_args()

    user_login = DEFAULT_LOGIN
    if args:
        user_login = args[0]

    if options.test and options.debug:
        print("sorry, can't do that")

    if options.schedule and (options.month, options.year) != (NOW.month, NOW.year):
        print("sorry, can't schedule for the past")

    elif options.test:
        import doctest

        test_globals = globals()
        test_globals.update({'BASE_SALARY': 9999, 'DEFAULT_LOGIN': 'Skitter'})
        results = doctest.testmod(optionflags=doctest.NORMALIZE_WHITESPACE, globs=test_globals)
        if not results.failed:
            print("All tests are green")

    else:
        run(user_login, options)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
