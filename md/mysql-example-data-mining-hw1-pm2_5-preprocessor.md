---
title: mysql example data-mining-hw1-pm2 5-preprocessor (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'data-mining-hw1-pm2 5-preprocessor'


Modules used in program: 
* `import time`
* `import _mysql`

## python data-mining-hw1-pm2 5-preprocessor

Python mysql example: data-mining-hw1-pm2 5-preprocessor

```python
import _mysql
import time

FILE_NAMES = [
    'C:\Users\go-creating\Desktop\data-mining-hw1\dataset\data.log-20160918',
    'C:\Users\go-creating\Desktop\data-mining-hw1\dataset\data.log-20160919',
    'C:\Users\go-creating\Desktop\data-mining-hw1\dataset\data.log-20160920',
    'C:\Users\go-creating\Desktop\data-mining-hw1\dataset\data.log-20160921',
    'C:\Users\go-creating\Desktop\data-mining-hw1\dataset\data.log-20160922',
    'C:\Users\go-creating\Desktop\data-mining-hw1\dataset\data.log-20160923',
    'C:\Users\go-creating\Desktop\data-mining-hw1\dataset\data.log-20160924',
    'C:\Users\go-creating\Desktop\data-mining-hw1\dataset\data.log-20160925',
]
SKIP_LINE_COUNT = 26

db=_mysql.connect(
    host='localhost',
    user='root',
    passwd='root',
    db='data_mining'
)

for filename in FILE_NAMES:
    with open(filename) as f:
        print('================')
        print('start parsing', filename)

        # line indicator
        i = SKIP_LINE_COUNT
        startTime = time.time()

        for _ in range(SKIP_LINE_COUNT):
            next(f)
        for line in f:
            # debugger
            i = i + 1
            if i % 10000 == 0:
                print(i, 'entries parsed')

            # remove \n character
            line = line[:-1]
            lineParts = line.split(' ')
            # avoid empty line
            if len(lineParts) == 2:
                entries = lineParts[1].split('|')[1:]
                fields = ['`topic_group`']
                values = ['\'' + lineParts[0].split('/')[1] + '\'']

                for entry in entries:
                    parts = entry.replace('\'', '').split('=')
                    # avoid empty entry
                    if len(parts) == 2:
                        field = '`' + parts[0] + '`'
                        # avoid `NaN` sensor value
                        value = ('\'' + parts[1] + '\'').replace("'NaN'", 'NULL')
                        # avoid duplicate fields
                        if field not in fields:
                            fields.append(field)
                            values.append(value)
                queryString = (
                    """
                    INSERT INTO raw (%s) VALUES (%s)
                    """ % (','.join(fields), ','.join(values))
                )
                try:
                    db.query(queryString)
                except Exception as err:
                    # error reporting
                    print('')
                    print('==================== %d line ====================' % i)
                    print(line)
                    print('')
                    print(fields)
                    print('')
                    print(values)
                    print('')
                    print(err)

        elapsedTime = time.time() - startTime
        print(i - SKIP_LINE_COUNT, 'lines parsed in ', elapsedTime, 'seconds')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
