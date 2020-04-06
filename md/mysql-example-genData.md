---
title: mysql example genData (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'genData'


Modules used in program: 
* `import _mysql`

## python genData

Python mysql example: genData

```python
from random import randrange
from random import choice
import _mysql

db = _mysql.connect(host="localhost", user="root", passwd="P@ssw0rd", db="pbnjout")

i = 1
while True:
    os = ["Unix", "Linux", "Windows Server"]
    status = ["success", "important"]
    spicked=choice(status)
    s2 = choice(status)
    s3 = choice(status)
    hostname = ["navy", "website", "forum", "army", "military", "ship", "air force", "database", "group", "network"]
    not_valid= [10,127,169,172,192] #private IP ranges will not be generated
    first=randrange(1,256)
    
    while first in not_valid:
        first=randrange(1,256)
    ip=".".join([str(first),str(randrange(1,256)),str(randrange(1,256)), str(randrange(1,256))])
    ospicked = choice(os)
    host = "-".join([choice(hostname), choice(hostname)])
    quer = """INSERT INTO `results` (`mid`,`ip`,`hostname`,`os`,`80`,`8080`,`443`) VALUES ("{0}","{1}","{2}","{3}","{4}","{5}","{6}");""".format(i, str(ip), str(host), str(ospicked), str(spicked), str(s2), str(s3))
    print(str(i) + " entries have been inserted into the database")
    db.query(quer)
    i = i + 1


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
