---
title: mysql example do (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'do'


Modules used in program: 
* `import re`

## python do

Python mysql example: do

```python
#coding=utf-8
import re


f = open("data.xml")
p2 = re.compile(r'<category.*label.*/>')
p1 = re.compile(r' <category.*label="reading-list"/>')
lines = f.readlines()
for index, line in enumerate(lines):
    if p2.search(line) or p1.search(line):
        lines[index] = ''

content = ''.join(lines)

f2 = open('new.xml','w')
f2.write(content)
f2.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
