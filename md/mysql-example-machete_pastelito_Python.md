---
title: mysql example machete pastelito Python (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'machete pastelito Python'


Modules used in program: 
* `import matplotlib.pyplot as plt`
* `import pandas as pd`
* `import numpy as np`
* `import mysql.connector as mariadb # --> para realizar conexion a una base de datos en mysql`

## python machete pastelito Python

Python mysql example: machete pastelito Python

```python
# importar librerias mas comunes de python
import mysql.connector as mariadb # --> para realizar conexion a una base de datos en mysql
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%config IPCompleter.greedy=True # --> auto completar codigo en jupyter
pd.set_option('display.max_rows', None) #--> para que imprima todo el contenido de Data Frame
# ----------------------------------------------

# codigo para realizar una consulta en una conexion de DB conpandas y guardar la tabla obtenida en un DataFrame
nombre_conexion = mariadb.connect(host='190.7.153.162', port='50005', user='nombre_user', password='********', database='nombre_db')
cursor = nombre_conexion.cursor()
nombre_df = pd.read_sql("consulta con la que se quiere traer la informacion de la DB", nombre_conexion)

# Para ejecutar una consulta, ya sea SELECT, UPDATE, TRUNCATE, etc ...
cursor.execute("Consulta SQL que se quiere ejecutar")

# Aqui cargamos archivo csv y le agregamos formate al campo de fecha, el 6 es el numero de la columna
df = pd.read_csv('nombre_archivo.csv', parse_dates=[6])


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
