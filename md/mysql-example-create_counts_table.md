---
title: mysql example create counts table (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'create counts table'

Functions in program: 
* `def read_input(input_):`
* `def main():`

Modules used in program: 
* `import csv`
* `import sys`
* `import fileinput`

## python create counts table

Python mysql example: create counts table

```python
#!/usr/bin/python3
"""
This will produce a table definition from the length count matrix
"""
from sqlalchemy.dialects import mysql
from sqlalchemy.schema import CreateTable
from sqlalchemy import Table, Column, String, MetaData

import fileinput
import sys
import csv

def main():
    table_name = 'import'

    if len(sys.argv) == 1:
        input_, output_ = fileinput.input(), sys.stdout
    elif len(sys.argv) == 2:
        input_, output_ = open(sys.argv[1]), sys.stdout
    elif len(sys.argv) == 3:
        table_name = sys.argv[2]
        input_, output_ = open(sys.argv[1]), open(sys.argv[2], 'w+')
    else:
        print("Usage: ./create-csv-table.py [input] [output]")
        sys.exit(1)

    reader = csv.reader(input_)
    next(reader) # discard "Field,Max Length"
    matrix = []
    for row in reader:
        matrix.append([row[0], int(row[1]) + 1])


    columns = [Column(n, String(l)) for n, l in matrix]
    table = CreateTable(Table(table_name, MetaData(),
        *columns
    ))

    output_.write(str(table.compile(dialect=mysql.dialect())))

if __name__ == '__main__':
    main()

"""
def read_input(input_):
    ""This function takes in an argument for csv.reader() and returns an
    instance of CSVCounts where counts property is matrix of max field lengths
    ""
    reader = csv.reader(input_)
    counts = CSVCounts()
    counts.establish_fields(next(reader))
    for data in reader:
        counts.compare_row(data)
    return counts

"""


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
