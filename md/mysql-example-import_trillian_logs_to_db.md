---
title: mysql example import trillian logs to db (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'import trillian logs to db'

Functions in program: 
* `def main(config):`
* `def worker(l,df,cl,cnf):`

Modules used in program: 
* `import re`
* `import os`
* `import io`
* `import itertools`
* `import multiprocessing`
* `import mysql.connector`
* `import xml.etree.ElementTree as ET`

## python import trillian logs to db

Python mysql example: import trillian logs to db

```python
from urllib.parse import unquote as unquote
import xml.etree.ElementTree as ET
import mysql.connector
import multiprocessing
import itertools
import io
import os
import re

def worker(l,df,cl,cnf):
    result={}
    for line in l:
        xmlstring='\n'.join(["<root>"]+re.findall(r'<.+?>',line)+["</root>"]) #.encode('unicode_escape')
        parsed_xml = ET.fromstring(xmlstring)
        for node in parsed_xml:
            xmlcols=list(node.attrib.keys())
            cols=[df[0]]+[cl[item] for item in xmlcols]
            if 'text' not in xmlcols:
                vals=sum((tuple([node.tag]),
                          tuple([node.attrib.get(xmlcols[item]) for item in range(len(cols[1:]))])), ())
            else:
                vals=sum((tuple([node.tag]), tuple([node.attrib.get(xmlcols[item]) if xmlcols[item] != 'text'
                                 else unquote(node.attrib.get(xmlcols[item]))for item in range(len(cols)-1)])), ())
            cols1=', '.join(cols)
            query =(f"INSERT INTO logs ({cols1}) "
                    "VALUES ("+("%s,"*len(vals))[:-1]+")")
            result[vals]=query
    # def ch(d, SIZE=int(numlines)):
    #     it = iter(d)
    #     for i in range(0, len(d), SIZE):
    #         yield {k:d[k] for k in itertools.islice(it, SIZE)}
    # chunks2=(({k:result[k] for k in itertools.islice(iter(result), numlines)},'jupyter','jupyternotebookuser') for i in range(0, len(result),numlines))

    cnx = mysql.connector.connect(user=cnf['db_user'], password=cnf['db_pass'], host=cnf['db_host'], database=cnf['db_db'], use_unicode = True, charset = cnf['charset'])
    cursor=cnx.cursor(buffered=True)
    for v,q in result.items():
        try:
            cursor.execute(q,v)
        except Exception as e:
            print(q)
            print(v)
            print(e)
    cnx.commit()
    cnx.close()

def main(config):
    conf={}
    conf.update(config)
    trilly_username=conf['trillian_username']
    trilly_log_path="C:\\Users\\"+conf['windows_username']+"\\AppData\\Roaming\\Trillian\\users\\"+trilly_username+"\\logs\\ASTRA\\Query\\"
    trilly_log_user=conf['trillian_target']+".xml"
    con = mysql.connector.connect(user=conf['db_user'], password=conf['db_pass'], host=conf['db_host'], database=conf['db_db'],use_unicode = True, charset = conf['charset'])
    cursor=con.cursor(buffered=True)
    cursor.execute("SHOW columns FROM "+conf['db_table'])
    dfcols = [column[0] for column in cursor.fetchall()][1:]     
    coldict=conf['coldict'] 
    fp=io.open(trilly_log_path+trilly_log_user+".xml",
    "r",encoding='utf-8-sig') 
    leng=len([1 for i in fp])
    fp.seek(0)
    lines = fp.readlines()
    numthreads = 6
    numlines = int(leng/(numthreads))
    chunks=((lines[aa:aa+numlines],dfcols,coldict,conf) for aa in range(0,len(lines),numlines))
    pool = multiprocessing.Pool(processes=numthreads)
    pool.starmap(worker,chunks)
    pool.close()
    con.close()

if __name__=="__main__":
    main({'db_user':'user','db_pass':'pass','db_host':'::1','db_db':'test','db_table':'logs','charset':'utf8mb4','windows_username':os.getlogin(),
          'trillian_target':'chatbuddy','trillian_username':'myaccount', 'coldict':{'entity':'entity','type':'type','time':'send_time',
          'ms':'ms','medium':'in_medium','to':'user_to','from':'user_from','from_display':'from_dspl','text':'msg_text','link':'msg_text'}})

# on "desc table;" it retuns:

# +-----------+------------+------+-----+---------+----------------+
# | Field     | Type       | Null | Key | Default | Extra          |
# +-----------+------------+------+-----+---------+----------------+
# | id        | int(11)    | NO   | PRI | NULL    | auto_increment |
# | entity    | text       | YES  |     | NULL    |                |
# | type      | text       | YES  |     | NULL    |                |
# | send_time | int(11)    | YES  | MUL | NULL    |                |
# | ms        | int(11)    | YES  |     | NULL    |                |
# | in_medium | text       | YES  |     | NULL    |                |
# | user_to   | text       | YES  |     | NULL    |                |
# | user_from | text       | YES  |     | NULL    |                |
# | from_dspl | text       | YES  |     | NULL    |                |
# | msg_text  | mediumtext | YES  |     | NULL    |                |
# +-----------+------------+------+-----+---------+----------------+

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
