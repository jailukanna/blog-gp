---
title: mysql example go (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'go'


Modules used in program: 
* `import sys`
* `import _mysql`

## python go

Python mysql example: go

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-

import _mysql
import sys

con = ""

try:
    con = _mysql.connect( host='mysql.ebi.ac.uk',
                          port=4085,
                          user='go_select',
                          passwd='amigo',
                          db='go_latest')

    con.query("""SELECT DISTINCT descendant.acc, descendant.name, descendant.term_type
               FROM
                 term
               INNER JOIN graph_path ON (term.id=graph_path.term1_id)
               INNER JOIN term AS descendant ON (descendant.id=graph_path.term2_id)
               WHERE term.name='nucleus' AND distance <> 0 ;""")

    result = con.use_result()

    print("MySQL version: %s" % \)
        result.fetch_row()
    
except _mysql.Error, e:
    print("Error %d: %s" % (e.args[0], e.args[1]))
    sys.exit(1)

finally:
    if con:
        con.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
