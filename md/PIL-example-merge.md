---
title: PIL example merge (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'merge'


Modules used in program: 
* `import subprocess`
* `import glob`
* `import os`

## python merge

Python PIL example: merge

```python
"""
Merge all the files from a given timestamp

Usage:

python merge.py

"""
import os
import glob
import subprocess
from sets import Set

prefix="MWP"
data_dir = os.curdir

out = []

# Find out the timespan
all_files = glob.glob(prefix + '*.tif')
all_dates = [x[4:11] for x in all_files]
date_set = Set(all_dates)
the_timespan = sorted(list(date_set))

# merge files by viewport
for time in the_timespan:
    output_name = "floods_%s.tif" % time
    output_file = os.path.join(data_dir, output_name)
    if os.path.exists(output_file):
        out.append(output_file)
    else:
        print(' - Merging %s' % output_file)
        files = glob.glob('%s*%s*' % (prefix, time))
        input_files = [os.path.join(data_dir, file) for file in files]
        subprocess.call(['gdal_merge.py',
  	     '-co', 'compress=packbits',
		     '-ot', 'Byte',
		     '-o', output_file]
		     + input_files,
		     stdout=open(os.devnull, 'w')
		     )
        out.append(output_file)

# create aggregated file for the past 26 days:
AlphaList=["A","B","C","D","E","F","G","H","I","J","K","L","M",
           "N","O","P","Q","R","S","T","U","V","W","X","Y","Z"]

calc_statement = ""
input = []

for i, f in enumerate(out):
    limit = len(AlphaList) - 1
    if i > limit:
        print('Only aggregating from %s to %s' % (the_timespan[0], the_timespan[i]))
        break

    letter = AlphaList[i]
    partial_statement = "1*(%s=3)" % letter
    if i == 0:
        calc_statement = partial_statement
    else:
        calc_statement = calc_statement + "+ " + partial_statement

    input.append("-%s" % letter)
    input.append(f)

calc_equation = "--calc=\"%s\"" % calc_statement
result = "--outfile=combined.tif"

subprocess.call(['gdal_calc.py',
     result, calc_equation]
     + input,
     stdout=open(os.devnull, 'w')
    )

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
