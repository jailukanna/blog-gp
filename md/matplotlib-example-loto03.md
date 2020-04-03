---
title: matplotlib example loto03 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'loto03'


Modules used in program: 
* `import csv`

## python loto03

Python matplotlib example: loto03

```python
import csv

csv_reader = csv.reader(open("loto6.csv","r",encoding='CP932'),delimiter=",",quotechar='"')
header = next(csv_reader)

print(' '.join(header))
for row in csv_reader:
  print(' '.join(row))



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
