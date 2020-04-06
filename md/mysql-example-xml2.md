---
title: mysql example xml2 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'xml2'


Modules used in program: 
* `import urllib.request`
* `import requests`
* `import pandas as pd`
* `import mysql.connector`

## python xml2

Python mysql example: xml2

```python
#cria string de conexão com banco de dados
import mysql.connector
import pandas as pd
import requests
from bs4 import BeautifulSoup
import urllib.request

cnx = mysql.connector.connect(user='abcd', password='jump****',
                              host='bdempresa.ccsuigoavhdp.sa-east-1.rds.amazonaws.com',
                              database='mgm')

# cria cursor para navegar por dados das tabelas
cursor = cnx.cursor()

# cria lista de pedidos de vendas
cursor.execute("select sales_number from mgm.mgm_sales")
rs = cursor.fetchall()
df_vtex = pd.DataFrame(rs)
df_vtex.columns = ["sales_number"]

x=0
tot = df_vtex['sales_number'].count()

while x < tot:
    
    s_order = df_vtex['sales_number'].iloc[x]
    api_url_vtex = 'https://api.vtexcrm.com.br/empresa/dataentities/VV/search?_fields=placa,createdIn&_where=order=' + s_order
    
  
    # Cria método GET
    req = urllib.request.urlopen(api_url_vtex)
    xml = BeautifulSoup(req,'xml')
         
    for item in xml.findAll('Value'):
        print(item)
    
    x += 1

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
