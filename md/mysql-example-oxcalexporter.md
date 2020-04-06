---
title: mysql example oxcalexporter (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'oxcalexporter'

Functions in program: 
* `def display(cal):`

Modules used in program: 
* `import mysql.connector as mc`
* `import os`
* `import sys`

## python oxcalexporter

Python mysql example: oxcalexporter

```python
#!/usr/bin/env python3

import sys
import os
from pick import pick
from icalendar import Calendar, Event, vDatetime
from datetime import datetime, date
from pytz import timezone
import mysql.connector as mc


#
# SQL Statements
#

# (user_id, mail)
sql_user = """
SELECT user.id, user.mail FROM user
WHERE user.id > 2;
"""

# (user_id, cal_id, cal_name)
sql_cals = """
SELECT oxtree.created_from, oxtree.fuid, oxtree.fname FROM oxfolder_tree AS oxtree
WHERE module = 2 AND type = 1 AND created_from > 2;
"""

# (user_id, cal_id, cal_name)
sql_events = """
SELECT * FROM calendar_event AS ce, calendar_attendee AS ca
WHERE ce.id = ca.event AND ca.folder = {cid};
"""

#
# Key mapping
#
k_map = [
    ('uid', 'uid'),
    ('dtstamp', 'timestamp'),
    #created
    #createdBy
    ('last-modified', 'modified'),
    #modifiedBy
    ('dtstart', 'start'),
    ('dtend', 'end'),
    ('allday', 'allDay'),
    ('rrule', 'rrule'),
    ('rdate', 'rDate'),
    ('exdate', 'exDate'),
    ('overriddendate', 'overriddenDate'),
    ('recurrence-id', 'recurrence'),
    ('sequence', 'sequence'),
    ('transp', 'transp'),
    ('class', 'class'),
    ('status', 'class'),
    #organizer
    ('summary', 'summary'),
    ('location', 'location'),
    ('description', 'description'),
    #categories
    #color
    #url
    #geo
    #rangefrom
    #rangeuntil
    #filename
    #extProp
    #account
    #event
    #entity
    #uri
    ('cn', 'cn')
    #folder
    #cuType
    #role
    #patStart
    #rsvp
    #comment
    #member
    #transp
    #extParam
]

stdtz = timezone('Europe/Berlin')

#
# MySQL Connection
#
try:
    cnx = mc.connect(user='daenara', passwd='seibel', host='localhost', db='oxdatabase_5')
    curs = cnx.cursor()

    curs.execute(sql_user)
    user = curs.fetchall()

    curs.execute(sql_cals)
    cals = curs.fetchall()

    sel = pick([mail for _,mail in user], 'Select users for export: ', multi_select=True, min_selection_count=1)
    sel = [uidx for _,uidx in sel]
    sel_user = [user[uidx][0] for uidx in sel]

    user_cals = [(dict(user)[cal[0]], cal[2], cal[1]) for cal in cals if cal[0] in sel_user]
    sel = pick([mail + ': ' + name for mail, name, _ in user_cals], 'Select calendars for export: ', multi_select=True, min_selection_count=1)
    sel = [cidx for _,cidx in sel]
    sel_cals = [user_cals[cidx][2] for cidx in sel]

    # loop over selected calendars:
    exp_cals = []
    for cid in sel_cals:
        curs.execute(sql_events.format(cid=cid))
        rows = curs.rowcount
        res = curs.fetchall()
        field_names = [i[0] for i in curs.description]
        events = [{k: v for (k, v) in zip(field_names, e)} for e in res]
        # single event for testing
        #events = [events[0], events[1]]

        cal = Calendar()
        cal.add('prodid', 'quizzis iCal-exporter')

        for e in events:
            event = Event()

            for kk in k_map:
                if e[kk[1]] is not None:
                    if 'dt' in kk[0] and kk[0] is not 'dtstamp' and kk[0] is not 'last-modified':
                        if kk[0] is 'dtstart':
                            tz = e['startTimezone']
                        else:
                            tz = e['endTimezone']
                        if tz is None:
                            tz = stdtz
                        else:
                            tz = timezone(tz)
                        if e['allDay'] == 1:
                            dt = date.fromisoformat(str(e[kk[1]])[:-9])
                            event.add(kk[0], dt)
                        else:
                            dt = datetime.fromisoformat(str(e[kk[1]]))
                            event.add(kk[0], dt.replace(tzinfo=tz))
                    elif kk[0] is 'dtstamp' or kk[1] is 'modified':
                        event.add(kk[0], datetime.fromtimestamp(e[kk[1]]/1000, stdtz))
                    elif kk[0] is 'rrule':
                        parts = e[kk[1]].split(';')
                        rrule = {k: v for k, v in [part.split('=') for part in parts]}
                        if 'UNTIL' in rrule.keys():
                            try:
                                rrule['UNTIL'] = vDatetime.from_ical(rrule['UNTIL'])
                            except ValueError as err:
                                print(err)
                                print(e)
                                sys.exit(1)
                        if 'BYDAY' in rrule.keys() and len(rrule['BYDAY']) > 2:
                            rrule['BYDAY'] = rrule['BYDAY'].split(',')
                        event.add(kk[0], rrule)
                    elif kk[0] is 'exdate':
                        if ',' in e[kk[1]]:
                            dt = [vDatetime.from_ical(t) for t in e[kk[1]].split(',')]
                            dt = tuple(dt)
                        else:
                            dt = vDatetime.from_ical(e[kk[1]])
                        event.add(kk[0], dt)
                    elif kk[0] is 'allday':
                        if e[kk[1]] == 1:
                            all = True
                        else:
                            all = False
                        event.add(kk[0], all)
                    else:
                        event[kk[0]] = e[kk[1]]

            cal.add_component(event)

        exp_cals.append(cal)

except mc.Error as e:
    print("Error %d: %s" % (e.args[0]. e.args[1]))
    sys.exit(1)
finally:
    curs.close()
    cnx.close()


def display(cal):
    return cal.to_ical().decode('utf-8').replace('\r\n', '\n').strip()


cdir = os.getcwd()
print('Exportet calendars:')
for idx,cal in enumerate(exp_cals):
    caldat = [cal for cal in user_cals if cal[2] == sel_cals[idx]][0]
    print(caldat[0:2])
    with open(os.path.join(cdir, caldat[0]+'_'+caldat[1]+'.ics'), 'wb') as f:
        f.write(cal.to_ical())

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
