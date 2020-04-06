---
title: mysql example dba (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'dba'


Modules used in program: 
* `import sqlconnect, os`

## python dba

Python mysql example: dba

```python
#!/usr/bin/python3

import sqlconnect, os

dbac = sqlconnect

ans = True
opc = True

while ans:

    print("""
    selecione la opcion que desea usar '1..5', escriba exit o precione '6' para salir del sistema.\n\n\a
    1-\t consultar residentes
    2-\t consultar visitantes
    3-\t consultar guardias
    4-\t consultar puertas
    5-\t consultar actividad
    6-\t salir/exit \n\n""")

    ans = str(input())

    if ans == "1":

        os.system('clear')

        while opc:

            print("""
            \n\t Seccion de residentes\n\tingrese la opcion que desea '1..4'\n\n
            1-\t agregar registro
            2-\t borrar registro
            3-\t consultar regitros
            4-\t salir al menu anterior""")

            opc = str(input())

            if opc == "1":

                #dbac.residentes.agregar(self=)
                pass

            elif opc == "2":
                pass
            elif opc == "3":
                pass
            elif opc == "4":
                print("\n\tregresando al menu anterior presione enter para continuar...")
                input()
                os.system('clear')
                break
            else:
                print("ingrese una opcion valida, presione enter para continuar...")
                input()
                os.system('clear')

    elif ans == "2":

        os.system('clear')

        while opc:

            print("""
            \n\t Seccion de visitantes\n\tingrese la opcion que desea '1..4'\n\n
            1-\t agregar registro
            2-\t borrar registro
            3-\t consultar regitros
            4-\t salir al menu anterior""")

            opc = str(input())

            if opc == "1":
                pass
            elif opc == "2":
                pass
            elif opc == "3":
                pass
            elif opc == "4":
                print("\n\tregresando al menu anterior presione enter para continuar...")
                input()
                os.system('clear')
                break
            else:
                print("ingrese una opcion valida, presione enter para continuar...")
                input()
                os.system('clear')

    elif ans == "3":

        os.system('clear')

        while opc:

            print("""
            \n\t Seccion de guardias\n\tingrese la opcion que desea '1..4'\n\n
            1-\t agregar registro
            2-\t borrar registro
            3-\t consultar regitros
            4-\t salir al menu anterior""")

            opc = str(input())

            if opc == "1":
                pass
            elif opc == "2":
                pass
            elif opc == "3":
                pass
            elif opc == "4":
                print("\n\tregresando al menu anterior presione enter para continuar...")
                input()
                os.system('clear')
                break
            else:
                print("ingrese una opcion valida, presione enter para continuar...")
                input()
                os.system('clear')

    elif ans == "4":

        os.system('clear')

        while opc:

            print("""
            \n\t Seccion de puertas\n\tingrese la opcion que desea '1..4'\n\n
            1-\t agregar registro
            2-\t borrar registro
            3-\t consultar regitros
            4-\t salir al menu anterior""")

            opc = str(input())

            if opc == "1":
                pass
            elif opc == "2":
                pass
            elif opc == "3":
                pass
            elif opc == "4":
                print("\n\tregresando al menu anterior presione enter para continuar...")
                input()
                os.system('clear')
                break
            else:
                print("ingrese una opcion valida, presione enter para continuar...")
                input()
                os.system('clear')

    elif ans == "5":

        os.system('clear')

        while opc:

            print("""
            \n\t Seccion de actividades\n\tingrese la opcion que desea '1..4'\n\n
            1-\t agregar registro
            2-\t borrar registro
            3-\t consultar regitros
            4-\t salir al menu anterior""")

            opc = str(input())

            if opc == "1":
                pass
            elif opc == "2":
                pass
            elif opc == "3":
                pass
            elif opc == "4":
                print("\n\tregresando al menu anterior presione enter para continuar...")
                input()
                os.system('clear')
                break
            else:
                print("ingrese una opcion valida, presione enter para continuar...")
                input()
                os.system('clear')

    elif ans == "6" or "exit":

        print("\n\thasta luego, presione enter para salir...")
        input()
        os.system('clear')
        raise SystemExit

    else:
        print("\a\nIngrese una opcion valida porfavor, presione enter para continuar...")
        input()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
