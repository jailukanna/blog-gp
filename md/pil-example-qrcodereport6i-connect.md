---
title: pil example qrcodereport6i-connect (snippet)
date: 2020-03-02
tags: ["python"]
---
Python pil example 'qrcodereport6i-connect'


Modules used in program: 
* `import cx_Oracle`
* `import qrcode`
* `import IPython.display`
* `import numpy as np`

## python qrcodereport6i-connect

Python pil example: qrcodereport6i-connect

```python
from PIL import Image, ImageDraw, ImageFont
from matplotlib.pyplot import imshow
import numpy as np
from PIL import Image
import IPython.display
import qrcode
import cx_Oracle

db = cx_Oracle.connect ('user','pw','connect string')
query = "select * from (select kd_kecamatan || kd_kelurahan || kd_blok || no_urut || kd_jns_op || substr(thn_pajak_sppt,3,2)  from sppt where kd_kecamatan = '120' and thn_pajak_sppt = '2019')"
cursor = db.cursor()
cursor.execute(query)

print('start')
i = 1
for row in cursor:
    print(i)
    createqr(row[0])
    i += 1
print('finish')

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
