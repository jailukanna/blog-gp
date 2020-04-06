---
title: mysql example ConverterClass (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'ConverterClass'


Modules used in program: 
* `import mysql`
* `import socket`

## python ConverterClass

Python mysql example: ConverterClass

```python
from talos_intel.connectors import Crete
import socket
import mysql

from terminaltables import AsciiTable

CHARSET='foo'

class CreteConverter(mysql.connector.conversion.MySQLConverter):
    """Return CRETE data sensible
    ref <https://stackoverflow.com/questions/27566078>
    """

    def row_to_python(self, row, fields):
        global CHARSET
        row = super().row_to_python(row, fields)

        def convert_type(col, field):
            (col_name, col_typ, ign0, ign1, ign2, ign3, null_ok, col_flags) = field

            if col_name in ('initiator_ip', 'responder_ip'):
                if type(col) == bytearray:
                    return socket.inet_ntoa(col)
                else:
                    return socket.inet_ntoa(col.encode(CHARSET))

            if type(col) == bytearray:
                try:
                    return col.decode(CHARSET)
                except UnicodeError:
                    return col
            else:
                return col


        return [convert_type(col, field) for col, field in zip(row, fields)]


query = "select * from security_intelligence_fact limit 1"
if False:
    crete = Crete(raw=True)
    cur, res = crete.run_sql_query(query)
    ret = []
    ret.append(cur.column_names)
    ret.append([repr(x) for x in next(res)])
    print(AsciiTable(ret).table)


if True:
    CHARSET='latin1'
    crete = Crete(converter_class=CreteConverter, charset=CHARSET, use_pure=True)
    cur, res = crete.run_sql_query(query)
    ret = []
    ret.append(cur.column_names)
    ret.append([repr(x) for x in next(res)])
    print(AsciiTable(ret).table)

if True:
    CHARSET='utf8'
    crete = Crete(converter_class=CreteConverter, charset=CHARSET, use_pure=True)
    cur, res = crete.run_sql_query(query)
    ret = []
    ret.append(cur.column_names)
    ret.append([repr(x) for x in next(res)])
    print(AsciiTable(ret).table)


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
