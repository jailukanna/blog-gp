---
title: socket example socket registrar (snippet)
date: 2020-03-01
tags: ["python"]
---
Python socket example 'socket registrar'

Functions in program: 
* `def crear_usuario(datos, es_servidor):`

Modules used in program: 
* `import os.path`

## python socket registrar

Python socket example: socket registrar

```python
manipular_archivos_rsa.py

import os.path


# datos = [correo, e, n] -> servidor
# datos = [correo, e, n, correo_cliente] -> cliente
def crear_usuario(datos, es_servidor):
	print("Crear usuario")
	if es_servidor:
		correo = datos[0]
		filename = "../archivos/sakura/usuarios.dat"
	else:
		correo_cliente = datos[3]
		del datos[-1] #remover el correo del usuario local
		filename = "../archivos/" + correo_cliente + "_usuarios.usuarios"
		correo = datos[0]
	cadena_datos = " ".join(datos) + "\n" #NOTA: buscar una mejor forma de poner el salto de linea
	archivo = open(filename, 'a')
	archivo.write(cadena_datos)
	archivo.close()
	
---------------------------------------------------------------------------------------------------------------------------------
sakura.py

	elif cadena[0] == "nuevo_correo":
		if cadena[2].isdigit() and cadena[3].isdigit():	#por si... acaso
			print("Crear o actualizar usuario")
			lista_de_datos = [cadena[1], cadena[2], cadena[3]]
			temporal = [lista_de_datos[0], None]
			datos_usuario_servidor = buscar_usuario(temporal, consulta_servidor) #buscar usuario en el archivo del servidor
			if datos_usuario_servidor == None: #si no existe, crear
					crear_usuario(lista_de_datos, consulta_servidor)
			else:								#si existe, actualizar
					actualizar_usuario(lista_de_datos, consulta_servidor)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
