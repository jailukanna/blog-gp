---
title: mysql example mantis tickets (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mantis tickets'


Modules used in program: 
* `import pytz`
* `import datetime`
* `import icalendar`
* `import mysql.connector`
* `import keyring`

## python mantis tickets

Python mysql example: mantis tickets

```python
#!/usr/bin/python

import keyring
import mysql.connector
import icalendar
import datetime
import pytz
from tzlocal import get_localzone

keyring_service = 'mantis_db_access'

qry_args = {
        'date_start':'2010-07-01',
        'date_end':'2020-08-01',
        'searchuser':'my_mantis_username',
        'searchemail':'my_personal@email,my_work@email'
    }

cnxargs = {'user':keyring.get_password(keyring_service,'username')}
cnxargs['password'] = keyring.get_password(keyring_service,cnxargs['user'])
cnxargs['database'] = keyring.get_password(keyring_service,'database') 
cnxargs['host'] = keyring.get_password(keyring_service,'host')
cnxargs['port'] = keyring.get_password(keyring_service,'port')

cnx = mysql.connector.connect(**cnxargs)

cursor = cnx.cursor(dictionary=True)

query = ("""
SELECT 
    mantis_bug_table.id,
    msg.date_submitted AS `when`,
    mantis_bug_table.status,
    mantis_project_table.name AS project,
        bugrep, noterep,
    GROUP_CONCAT(mantis_tag_table.name) AS tags,
    summary,
    if (note is NULL, CONCAT(mantis_bug_text_table.description, steps_to_reproduce, additional_information), note) as note,
    bughand,
    monuser.username AS monitoring
FROM
    mantis_bug_table
        JOIN
    (SELECT DISTINCT
        mantis_bug_table.id AS bug_id,
            NULL AS noterep,
            mut_bugrep.username AS bugrep,
            mut_bughand.username AS bughand,
            mantis_bug_table.date_submitted,
            NULL as note
    FROM
        mantis_bug_table
    JOIN mantis_bugnote_table ON mantis_bug_table.id = mantis_bugnote_table.bug_id
    JOIN mantis_user_table AS mut_noterep ON mut_noterep.id = mantis_bugnote_table.reporter_id
    JOIN mantis_user_table AS mut_bugrep ON mut_bugrep.id = mantis_bug_table.reporter_id
    JOIN mantis_user_table AS mut_bughand ON mut_bughand.id = mantis_bug_table.handler_id
    JOIN mantis_project_table ON mantis_project_table.id = mantis_bug_table.project_id
    WHERE
        mantis_bug_table.date_submitted > UNIX_TIMESTAMP(%(date_start)s)
            AND mantis_bug_table.date_submitted < UNIX_TIMESTAMP(%(date_end)s)
            AND (FIND_IN_SET(mut_noterep.username, %(searchuser)s) > 0
            OR FIND_IN_SET(mut_noterep.email, %(searchemail)s) > 0
            OR FIND_IN_SET(mut_bugrep.username, %(searchuser)s) > 0
            OR FIND_IN_SET(mut_bugrep.email, %(searchemail)s) > 0
            OR FIND_IN_SET(mut_bughand.username, %(searchuser)s) > 0
            OR FIND_IN_SET(mut_bughand.email, %(searchemail)s) > 0
            OR FIND_IN_SET(mantis_project_table.name, @searchprojects)) UNION SELECT DISTINCT
        mantis_bug_table.id AS id,
            mut_noterep.username AS noterep,
            mut_bugrep.username AS bugrep,
            mut_bughand.username AS bughand,
            mantis_bugnote_table.date_submitted,
            mantis_bugnote_text_table.note
    FROM
        mantis_bug_table
    JOIN mantis_bugnote_table ON mantis_bug_table.id = mantis_bugnote_table.bug_id
    JOIN mantis_user_table AS mut_noterep ON mut_noterep.id = mantis_bugnote_table.reporter_id
    JOIN mantis_user_table AS mut_bugrep ON mut_bugrep.id = mantis_bug_table.reporter_id
    JOIN mantis_user_table AS mut_bughand ON mut_bughand.id = mantis_bug_table.handler_id
    JOIN mantis_project_table ON mantis_project_table.id = mantis_bug_table.project_id
    JOIN mantis_bugnote_text_table ON mantis_bugnote_table.bugnote_text_id = mantis_bugnote_text_table.id
    WHERE
        mantis_bugnote_table.date_submitted >= UNIX_TIMESTAMP(%(date_start)s)
            AND mantis_bugnote_table.date_submitted <= UNIX_TIMESTAMP(%(date_end)s)
            AND (FIND_IN_SET(mut_noterep.username, %(searchuser)s) > 0
            OR FIND_IN_SET(mut_noterep.email, %(searchemail)s) > 0
            OR FIND_IN_SET(mut_bugrep.username, %(searchuser)s) > 0
            OR FIND_IN_SET(mut_bugrep.email, %(searchemail)s) > 0
            OR FIND_IN_SET(mut_bughand.username, %(searchuser)s) > 0
            OR FIND_IN_SET(mut_bughand.email, %(searchemail)s) > 0
            OR FIND_IN_SET(mantis_project_table.name, @searchprojects))) AS msg ON mantis_bug_table.id = msg.bug_id
        JOIN
    mantis_project_table ON mantis_project_table.id = mantis_bug_table.project_id
                JOIN mantis_bug_text_table ON mantis_bug_table.bug_text_id = mantis_bug_text_table.id
        LEFT OUTER JOIN
    mantis_bug_tag_table ON mantis_bug_table.id = mantis_bug_tag_table.bug_id
        LEFT OUTER JOIN
    mantis_tag_table ON mantis_bug_tag_table.tag_id = mantis_tag_table.id
        LEFT OUTER JOIN
    mantis_bug_monitor_table ON mantis_bug_table.id = mantis_bug_monitor_table.bug_id
        LEFT OUTER JOIN
    mantis_user_table AS monuser ON mantis_bug_monitor_table.user_id = monuser.id
GROUP BY monuser.username, msg.date_submitted
ORDER BY msg.date_submitted;""")

cursor.execute(query, qry_args)

cal = icalendar.Calendar()

cal.add('prodid', '-//mantis to ical//mikemol/')
cal.add('version', '2.0')

for row in cursor:
    when = datetime.datetime.fromtimestamp(row['when'], get_localzone()).astimezone(pytz.timezone('UTC'))
    event = icalendar.Event()
    event['DTSTAMP'] = when.strftime("%Y%m%dT%H%M%SZ")
    event['CREATED'] = event['DTSTAMP']
    event['DTSTART'] = event['DTSTAMP']
    event['DURATION'] = 'PT5M'
    event['UID'] = "%(id)s@%(when)s" % row
    event['LAST-MODIFIED'] = event['DTSTAMP']
    event['CLASS'] = 'PRIVATE'
    event['CATEGORIES'] = "%s,%s" % (row['project'], row['tags']) if row['tags'] is not None else row['project']
    event['ORGANIZER'] = "MAILTO:support@example.com?subject=%%5B%(id)s%%5D" % row
    event['SUMMARY'] = "[%(id)s] [%(project)s] %(summary)s" % row

    desc = "http://mantis.example.com/view.php?id=%(id)s\n\n" % row
    desc = desc + row['note']
    event['DESCRIPTION'] = desc

    cal.add_component(event)

print(cal.to_ical().decode('utf-8'))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
