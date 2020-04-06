---
title: mysql example workshop (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'workshop'


Modules used in program: 
* `import logging`
* `import re`
* `import luigi`

## python workshop

Python mysql example: workshop

```python
from edx.analytics.tasks.mysql_load import MysqlInsertTask

from edx.analytics.tasks.url import get_target_from_url, url_path_join, ExternalURL
from edx.analytics.tasks.pathutil import EventLogSelectionMixin
from edx.analytics.tasks.mapreduce import MapReduceJobTask
from edx.analytics.tasks.util import eventlog
import luigi
import re
from edx.analytics.tasks.util.record import Record, StringField, IntegerField
import logging

log = logging.getLogger(__name__)


class HelloWorldTask(luigi.Task):

    input_url = luigi.Parameter()
    output_url = luigi.Parameter()

    def requires(self):
        return ExternalURL(self.input_url)

    def run(self):
        with self.input().open(mode='r') as input_file:
            with self.output().open(mode='w') as output_file:
                for line in input_file:
                    output_file.write('Hello ' + line)

    def output(self):
        return get_target_from_url(self.output_url)


class HelloWorldMapReduceTask(EventLogSelectionMixin, MapReduceJobTask):

    output_root = luigi.Parameter()

    def mapper(self, line):
        value = self.get_event_and_date_string(line)
        if value is None:
            return
        event, _date_string = value

        key = 'hello ' + event.get('event_type', 'UNKNOWN')
        yield key, 1

    def reducer(self, key, values):
        yield key, sum(values)

    def output(self):
        return get_target_from_url(url_path_join(self.output_root, self.interval.to_string()) + '/')


class ViewDistribution(EventLogSelectionMixin, MapReduceJobTask):

    SUBSECTION_ACCESSED_PATTERN = r'/courses/.*?courseware/([^/]+)/([^/]+)/.*$'

    output_root = luigi.Parameter()

    def mapper(self, line):
        value = self.get_event_and_date_string(line)
        if value is None:
            return
        event, _date_string = value

        event_type = event.get('event_type')
        if event_type is None:
            return

        # Challenge #1
        # Use the SUBSECTION_ACCESSED_PATTERN to extract the section and subsection from the URL. The first match will
        # be the section and the second will be the subsection.
        m = re.match(self.SUBSECTION_ACCESSED_PATTERN, event_type)
        if not m:
            return
        section, subsection = m.group(1, 2)

        username = event.get('username', '').strip()
        if not username:
            return

        course_id = eventlog.get_course_id(event)
        if not course_id:
            return

        # Challenge #2
        # What should this method yield?
        yield (course_id, section, subsection), (username)

    def reducer(self, key, values):
        # Challenge #2 continued
        # I recommend assigning each element of your key to a variable here: foo, bar = key
        course_id, section, subsection = key

        # Challenge 3
        # Compute the total number of views for each (section, subsection) as well as the unique number of users who
        # viewed that subsection.
        unique_usernames = set()
        total_views = 0
        for username in values:
            unique_usernames.add(username)
            total_views += 1
        unique_user_views = len(unique_usernames)

        yield ViewRecord(
            course_id=course_id,
            section=section,
            subsection=subsection,
            unique_user_views=unique_user_views,
            total_views=total_views
        ).to_string_tuple()

    def output(self):
        return get_target_from_url(url_path_join(self.output_root, self.interval.to_string()) + '/')


class ViewRecord(Record):
    course_id = StringField(length=255, nullable=False)
    section = StringField(length=255, nullable=False)
    subsection = StringField(length=255, nullable=False)
    unique_user_views = IntegerField()
    total_views = IntegerField()


class ViewDistributionMysqlTask(MysqlInsertTask):

    output_root = luigi.Parameter()
    interval = luigi.DateIntervalParameter()
    n_reduce_tasks = luigi.IntParameter(significant=False)

    @property
    def table(self):
        return 'content_views'

    @property
    def columns(self):
        return ViewRecord.get_sql_schema()

    @property
    def indexes(self):
        return [
            ('course_id',)
        ]

    @property
    def insert_source_task(self):
        return ViewDistribution(
            interval=self.interval,
            output_root=self.output_root,
            n_reduce_tasks=self.n_reduce_tasks
        )


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
