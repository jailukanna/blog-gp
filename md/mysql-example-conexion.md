---
title: mysql example conexion (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'conexion'


Modules used in program: 
* `import mysql.connector`

## python conexion

Python mysql example: conexion

```python
import mysql.connector

# Diccionario Datos de Conexion
config_mysql = {
    'user': 'root',
    'password': '',
    'host': 'localhost',
    'database': 'principal',
}

# Objeto Conexion
conexion_mysql = mysql.connector.connect(**config_mysql)

# Objeto cursor     // permitira hacer las consultas
cursor = conexion_mysql.cursor()

#   Ejecucion de la consulta
cursor.execute("SELECT * FROM servicios_io_f limit 0,10")

for (Campo1, Campo2, Campo3, Campo2, Campo3) in cursor:
    print("Campo1: " + Campo1 + ", Campo2: " + str(Campo2) + ", Campo3: " + str(Campo3))

# Cerramos la variable encargada de las consultas
cursor.close()
#   cierre de conexion
conexion_mysql.close()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
