---
title: mysql example generateHeatmapData (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'generateHeatmapData'

Functions in program: 
* `def runQuery(query):`

Modules used in program: 
* `import sys`
* `import progressbar`
* `import mysql.connector`

## python generateHeatmapData

Python mysql example: generateHeatmapData

```python
import mysql.connector
import progressbar
import sys

def runQuery(query):
    c.execute(query)
    (res,) = c.fetchone()
    return res

if len(sys.argv)<2:
    print("No tolerance given")
    exit()

tolerance=sys.argv[1]

# Boundaries based on the 99th percentile for character count and number of snippets
MAX_CHARS = 17000
CHAR_STEP = 850
MAX_SNIPPETS = 80
SNIPPET_STEP= 4

heatmap_data = []
# MySQL conifig
config = {
  'user': 'root',
  'password': 'password',
  'host': 'localhost',
  'database': 'jupyter',
  'raise_on_warnings': True
}

conn = mysql.connector.connect(**config)
c = conn.cursor(buffered=True)

# Progressbar stuff
counter = 0
bar = progressbar.ProgressBar(maxval=400,
                               widgets=[progressbar.Bar('=', '[', ']'), ' ',
                               progressbar.Percentage()])
bar.start()

# Iterate over the various ranges of snippets and character count
for j in range(0,MAX_SNIPPETS-SNIPPET_STEP, SNIPPET_STEP):
    snippet_lower_bound = j
    snippet_upper_bound = j+10

    for i in range(0,MAX_CHARS,CHAR_STEP):
        counter += 1
        bar.update(counter)

        char_lower_bound = i
        char_upper_bound = (i+CHAR_STEP)

        # Running with duplication tolerance larger than 1 will include complete clones
        query = '''SELECT AVG(DuplicationRatio)\
                   FROM Notebooks\
                   WHERE\
                        NoOfSnippets BETWEEN {} AND {}\
                        AND NoOfChars BETWEEN {} AND {}\
                        AND DuplicationRatio<{};'''.format(snippet_lower_bound,
                                                               snippet_upper_bound,
                                                               char_lower_bound,
                                                               char_upper_bound,
                                                               tolerance)

        result = runQuery(query)

        # Missing results are read as "NaN" by seaborn
        if not result:
            result = "NaN"
        range_result = "%s,%s,%s" % (snippet_upper_bound, char_upper_bound, result)
        heatmap_data.append(range_result)

bar.finish()
conn.close()

headers = "NumberOfSnippets,Chars,DuplicateFraction"
# Save result to file
with open("csv/heatmapData_{}.csv".format(tolerance), "w") as f:
    f.write(headers+'\n')
    for d in heatmap_data:
        f.write(d+'\n')

print("Finishes for duplication tolerance of {}".format(tolerance))




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
