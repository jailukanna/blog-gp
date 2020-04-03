---
title: matplotlib example csv open (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'csv open'


Modules used in program: 
* `import csv`

## python csv open

Python matplotlib example: csv open

```python
import csv

csv_reader = csv.reader(open("loto6.csv","r",encoding='cp932'))
#csv_reader = csv.reader(open("loto6.csv","r",encoding='cp932'),delimiter=",",quotechar='"')

header = next(csv_reader)
print('\n'.join(header))

for row in csv_reader:
	c = row[3:-12]
	b = list(map(int,c))#文字列を変換
  
	print(b)
  #print(row)



```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
