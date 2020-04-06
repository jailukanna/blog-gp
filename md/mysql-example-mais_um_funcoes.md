---
title: mysql example mais um funcoes (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'mais um funcoes'

Functions in program: 
* `def ask_altura():`
* `def ask_sexo():`
* `def ask_dta():`
* `def ask_name(msg = "DÍGITE O NOME: "):`
* `def validar_dia():`
* `def validar_mes():`
* `def validar_ano():`
* `def ask_int(msg, msg2):`

## python mais um funcoes

Python mysql example: mais um funcoes

```python
def ask_int(msg, msg2):
    try:
        ask = int(input(msg))
        return ask
    except(ValueError):
        print(msg2)
        return ask_int(msg,msg2)

def validar_ano():
    ano = ask_int("DIGITE O ANO DE NASCIMENTO: ",
                  "CONTEUDO DIGITADO INVALIDO. TENTE NOVAMENTE.")

    num = len(str(ano))

    if  num == 4:
        return ano
    else:
        print("Ano invalido. Tente novamente.")
        return validar_ano()

def validar_mes():
    mes = ask_int("DIGITE O MÊS DE NASCIMENTO: ",
                  "CONTEUDO DIGITADO INVALIDO. TENTE NOVAMENTE.")

    if mes <= 12 and mes >= 1:
        return mes
    else:
        print("DIA INVALIDO, TENTE NOVAMENTE.")
        return validar_mes()

def validar_dia():
    dia = ask_int("DIGITE O MÊS DE NASCIMENTO: ",
                  "CONTEUDO DIGITADO INVALIDO. TENTE NOVAMENTE.")

    if dia <= 31 and dia >= 1:
        return dia
    else:
        print("DIA INVALIDO, TENTE NOVAMENTE.")
        return validar_dia()

def ask_name(msg = "DÍGITE O NOME: "):
    nome = input(msg)

    if len(nome) > 50:
        input("O nome utrapassa a quantidade maxima de letras."+ \
              "-----------------------------------" + \
              "precione [ENTER] e tente novamente.")
        return ask_name()
    return nome

def ask_dta():
    ano = validar_ano()
    mes = validar_mes()
    dia = validar_dia()
    return "{}-{}-{}".format(str(ano), str(mes), str(dia))

def ask_sexo():
    print("SEXO ")
    sexo  = input("DIGITE:\n"+ \
                  " F - para feminino\n"+ \
                  " M - para masculino\n"+ \
                  " O - para outros\n")
    sexo = sexo.upper()

    if sexo == "0" or sexo == "F" or sexo == "M":
        return sexo
    else:
        print("OPÇÃO INVALIDA. TENTE NOVAMENTE.")
        return ask_sexo()

def ask_altura():
    altura = ask_int("DIGITE SUA ALTURA EM CMs: ",
                      "Valor invalido. Tente novamente.")

    if altura >= 10 and altura <= 250:
        return altura
    else:
        print("Só é aceito valores entre 10 a 250.\n"+\
              "tente novamente.")
        return  ask_altura()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
