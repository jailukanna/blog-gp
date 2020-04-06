---
title: mysql example CloudSQLSource (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'CloudSQLSource'


Modules used in program: 
* `import apache_beam as beam`
* `import mysql.connector`
* `import threading`

## python CloudSQLSource

Python mysql example: CloudSQLSource

```python
import threading

import mysql.connector

import apache_beam as beam

class CloudSQLSource(beam.io.iobase.BoundedSource):

    def __init__(self, node):
        sql = mysql.connector.connect(user='', password='',
                                      host='127.0.0.1',
                                      database='', port=3306)
        
        cursor = sql.cursor(dictionary=True)
        self._node = node
        cursor.execute(
            "SELECT COUNT(*) FROM `TABLE`")
        result = cursor.fetchone()
        self._count = result['COUNT(*)']

        cursor.close()
        sql.close()

    def estimate_size(self):
        return self._count

    def get_range_tracker(self, start_position, stop_position):
        logging.warning('------ get range tracker ---------')
        import apache_beam as local_beam
        print(start_position, stop_position)
        if start_position is None:
            start_position = 0
        if stop_position is None:
            stop_position = self._count

        return local_beam.io.range_trackers.OffsetRangeTracker(start_position, stop_position)

    def read(self, range_tracker):
        logging.warning('------------   read   _------------------')
        start = range_tracker.start_position()
        if range_tracker.start_position() is None:
            start = 0

        count = range_tracker.stop_position()
        if count is None:
            count = self._count
        count = count - start

        print(start)
        print(count)

        if not range_tracker.try_claim(start):
            logging.warning('claim error')
            return

        sql = mysql.connector.connect(user='', password='',
                                        host='127.0.0.1',
                                        database='', port=3306)
        cursor = sql.cursor(dictionary=True)

        cursor.execute(
            'SELECT * FROM `TABLE` LIMIT %s, %s ', (start, count))

        results = cursor.fetchall()
        for row in results:
            yield row

        cursor.close()
        sql.close()

    def split(self, desired_bundle_size, start_position=None,
              stop_position=None):

        logging.warning('----- split -------')
        logging.warning('%s %s %s', desired_bundle_size,
                        start_position, stop_position)

        import apache_beam as local_beam

        if start_position is None:
            start_position = 0
        if stop_position is None:
            stop_position = self._count

        bundle_start = start_position
        while bundle_start < self._count:
            bundle_stop = max(self._count, bundle_start + desired_bundle_size)
            yield local_beam.io.iobase.SourceBundle(weight=(bundle_stop - bundle_start),
                                                    source=self,
                                                    start_position=bundle_start,
                                                    stop_position=bundle_stop)
            bundle_start = bundle_stop



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
