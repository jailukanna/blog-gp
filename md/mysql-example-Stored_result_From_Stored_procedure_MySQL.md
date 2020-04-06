---
title: mysql example Stored result From Stored procedure MySQL (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'Stored result From Stored procedure MySQL'


Modules used in program: 
* `import mysql.connector`
* `import datetime`

## python Stored result From Stored procedure MySQL

Python mysql example: Stored result From Stored procedure MySQL

```python
import datetime
import mysql.connector
from openpyxl import Workbook
from mysql.connector import Error

data = (datetime.date(2020, 1, 31))
print(f"Date: {data} SELECTED")

header = [
    ("Data", "Cliente", "Descricao", "Quantidade", "Preco_Unitario", "Valor_de_Mercado")
]

arquivo_excel = Workbook()
planilha1 = arquivo_excel.active
print("Excel File: OPENED")

try:
    connection = mysql.connector.connect(host='127.0.0.1',
                                         database='***',
                                         user='***',
                                         password='***')
    print("Database: CONNECTED")
    cursor = connection.cursor()
    cursor.callproc('MJ_posicao_carteira', (data,))
    print("Stored Procedure: CALLED SUCCESSFULLY")

    for linha in header:
        planilha1.append(linha)

    for j in cursor.stored_results():
        for row in j:
            planilha1.append(row)

    print("Downloading Datas")


#except Error as e:
#    print("Error reading data from MySQL table", e)
finally:
    if (connection.is_connected()):
        connection.close()
        cursor.close()
        print("MySQL connection is closed")

arquivo_excel.save("C:/Users/***/Documents/Posicao_Carteira.xlsx")

print("File Posicao_Carteira.xlsx CREATED")

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
