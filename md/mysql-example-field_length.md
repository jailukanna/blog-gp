---
title: mysql example field length (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'field length'

Functions in program: 
* `def write_output(output_, counts):`
* `def read_input(input_):`
* `def main():`

Modules used in program: 
* `import sys`
* `import fileinput`
* `import csv`

## python field length

Python mysql example: field length

```python
#!/usr/bin/python3
"""
This will calculate max field lengths
"""
import csv
import fileinput
import sys
from collections import defaultdict

def main():
    input_, output_ = None, None

    # Usage: ./field-length.py [input] [output]
    if len(sys.argv) == 1:
        input_, output_ = fileinput.input(), sys.stdout
    elif len(sys.argv) == 2:
        input_, output_ = open(sys.argv[1]), sys.stdout
    elif len(sys.argv) == 3:
        input_, output_ = open(sys.argv[1]), open(sys.argv[2], 'w+')
    else:
        print("Usage: ./field_length.py [input] [output]")
        sys.exit(1)

    counts = read_input(input_)
    write_output(output_, counts)

def read_input(input_):
    """This function takes in an argument for csv.reader() and returns an
    instance of CSVCounts where counts property is matrix of max field lengths
    """
    reader = csv.reader(input_)
    counts = CSVCounts()
    counts.establish_fields(next(reader))
    for data in reader:
        counts.compare_row(data)
    return counts

def write_output(output_, counts):
    # write output
    writer = csv.writer(output_)
    max_lengths = counts.counts
    writer.writerow(["Field", "Max Length"])
    writer.writerows(max_lengths)

class CSVCounts(object):
    def __init__(self):
        super(CSVCounts, self).__init__()

        self.field_names = {}
        self._counts = defaultdict(int)

    def establish_fields(self, fields):
        self.field_names = fields

    def compare_row(self, row):
        for field_index, value in enumerate(row):
            new_count = len(value)
            if self._counts[field_index] < new_count:
                self._counts[field_index] = new_count

    @property
    def counts(self):
        counts = []
        for field_index, field in enumerate(self.field_names):
            counts.append([field, self._counts[field_index]])
        return counts
    
    @property
    def fields(self):
        return self.field_names

if __name__ == '__main__':
    try:
        main()
    except StopIteration:
        print("Input File is empty")


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
