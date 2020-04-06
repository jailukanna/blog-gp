---
title: mysql example export (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'export'


Modules used in program: 
* `import mysql.connector`

## python export

Python mysql example: export

```python
# -*- coding: utf-8 -*-
import mysql.connector
from docx import Document


db = 'lanyun'
host = 'localhost'
user = 'root'
password = 'lanyun'
port = 3306

conn = mysql.connector.connect(host=host, user=user, passwd=password, db=db, port=port)
cursor = conn.cursor()
cursor.execute("select table_name,table_comment from information_schema.tables where table_schema = 'lanyun'")
document = Document()

for (table_name, table_comment) in cursor.fetchall():
    document.add_heading(table_name, level=1)
    document.add_paragraph(table_comment)
    table = document.add_table(rows=1, cols=8, style='Table Grid')
    hdr_cells = table.rows[0].cells
    hdr_cells[0].text = u'编号'
    hdr_cells[1].text = u'字段名'
    hdr_cells[2].text = u'字段'
    hdr_cells[3].text = u'类型'
    hdr_cells[4].text = u'主键'
    hdr_cells[5].text = u'外键'
    hdr_cells[6].text = u'必填'
    hdr_cells[7].text = u'备注'
    cursor.execute("SHOW FULL FIELDS FROM lanyun.%s" % table_name)
    i = 1
    for (field, type, _, isnull, key, _, _, _, comment) in cursor.fetchall():
        row_cells = table.add_row().cells
        row_cells[0].text = str(i)
        i = i + 1
        row_cells[1].text = ''
        row_cells[2].text = field
        row_cells[3].text = type
        row_cells[4].text = u'是' if key == 'PRI' else u'否'
        row_cells[5].text = u'是' if key == 'MUL' else u'否'
        row_cells[6].text = u'是' if isnull == 'YES' else u'否'
        row_cells[7].text = comment

document.save('db.docx')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
