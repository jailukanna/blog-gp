---
title: mysql example crud (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'crud'

Functions in program: 
* `def deletar():`
* `def update():`
* `def read():`
* `def create():`
* `def ask_altura():`
* `def ask_sexo():`
* `def ask_dta():`
* `def ask_name(msg = "DÍGITE O NOME: "):`
* `def validar_dia():`
* `def validar_mes():`
* `def validar_ano():`
* `def ask_int(msg, msg2):`

Modules used in program: 
* `import datetime`
* `import mysql.connector`

## python crud

Python mysql example: crud

```python
import mysql.connector
import datetime
from mais_um import funcoes

cnx = mysql.connector.connect(user='root', password='root',
                              host='127.0.0.1', database='test')

#funcões
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

    if num == 4:
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
    dia = ask_int("DIGITE O DIA DE NASCIMENTO: ",
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

    try:
        data = datetime.date(ano, mes, dia)
        return "{}-{}-{}".format(str(ano), str(mes), str(dia))
    except(ValueError):
        print("Data inexistente, tente novamente.")
        return ask_dta()

def ask_sexo():
    print("SEXO ")
    sexo  = input("DIGITE:\n"+ \
                  " F - para feminino\n"+ \
                  " M - para masculino\n"+ \
                  " O - para outros\n")
    sexo = sexo.upper()

    if sexo == "O" or sexo == "F" or sexo == "M":
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

#fim

def create():
   name   = ask_name()
   data   = ask_dta()
   sexo   = ask_sexo()
   altura = ask_altura()

   sql = "INSERT INTO  tb_pessoas(nme_pessoa, dta_nasc_pessoa, sex_pessoa, num_altura_pessoa) VALUES (%s, %s, %s, %s);"
   cursor = cnx.cursor()
   cursor.execute(sql, (name, data, sexo, altura))
   cnx.commit()
   cursor.close()
   input("----------------------------------------------"+ \
         "CADASTRO CONCLUIDO!\n"+ \
         "----------------------------------------------"+\
         "presione [ENTER]")


   return

def read():
    opcao = ask_int("DIGITE:\n" + \
                    "  1 - para todos os cadastros\n"+ \
                    "  2 - para procurar por nome.\n",
                    "CONTEUDO DIGITADO INVALIDO. TENTE NOVAMENTE.")
    if opcao == 1:
        sql = "SELECT * FROM tb_pessoas;"
        cursor = cnx.cursor()
        cursor.execute(sql, )

    elif opcao == 2:
        name = ask_name()
        sql = "SELECT * FROM tb_pessoas WHERE nme_pessoa LIKE CONCAT('%', %s, '%');"

        cursor = cnx.cursor()
        cursor.execute(sql, (name,))
    else:
        print("OPÇÃO INVALIDA! TENTE NOVAMENTE.")
        return read()


    for (idt, nome, data, sexo, altura) in cursor:
        print(idt, nome, data, sexo, altura)
    cursor.close()
    print("-------------------------------------------------")
    input("[Enter] para menu")


    return

def update():
   idt = ask_int("Qual a matricula para alterar? ",
                 "lembrece que a matricula é composta de números.\n"+ \
                 " Tente novamente.\n\n")

   sql    = ("SELECT * from tb_pessoas WHERE idt_pessoa=%s;")

   cursor = cnx.cursor()
   cursor.execute(sql, (idt,))
   (idt, nme, dta, sex, num_altura) = cursor.fetchone()
   cursor.close()

   nome   = ask_name("Digite o novo nome do(a) [" + nme + "]: ")
   data   = ask_dta()
   sexo   = ask_sexo()
   altura = ask_altura()

   sql = "UPDATE tb_pessoas SET nme_pessoa=%s, dta_nasc_pessoa=%s, "+ \
         "sex_pessoa=%s, num_altura_pessoa=%s WHERE idt_pessoa=%s;"
   cursor = cnx.cursor()
   cursor.execute(sql, (nome, data, sexo, altura, idt))
   cnx.commit()
   cursor.close()

   print("-------------------------------------------------")
   input("[Enter] para menu")
   return

def deletar():
    idt = ask_int("DIGITE A NUMERO DO CADASTRO: ",
                  "Só é aceito numeros inteiros.\n Tente novamente.")

    sql = ("SELECT * from tb_pessoas WHERE idt_pessoa=%s;")

    cursor = cnx.cursor()
    cursor.execute(sql, (idt,))
    (idt, nme, dta, sex, altura) = cursor.fetchone()
    cursor.close()

    print("Nome: " + nme)
    print("Matricula: ", idt)

    confirmacao = input("EXCLUIR [S/N]:")

    if confirmacao.upper() == "S":
        sql = "DELETE FROM tb_pessoas WHERE idt_pessoa=%s;"
        cursor = cnx.cursor()
        cursor.execute(sql, (idt,))
        cnx.commit()
        cursor.close()
        print("Excluido.")

    print("-------------------------------------------------")
    input("[Enter] para menu")

    return

text = True
while text:
   print("BEM VINDO AO BANCO DE DADOS!\n\n")
   try:
      menu = int(input("DÍGITE:\n  1 - para criar cadastro\n" + \
                       "  2 - para pesquisar cadastro\n" + \
                       "  3 - para alterar cadastro\n" + \
                       "  4 - para excluir cadastro\n" + \
                       "  5 - para sair.\n"))

   except(ValueError):
      print("Objeto digitado não coresponde com nenhuma opção\n"+ \
            "TENTE NOVAMENTE.")

   if menu == 1:
      create()
   elif menu == 2:
      read()
   elif menu == 3:
      update()
   elif menu == 4:
      deletar()
   elif menu == 5:
      print("FIM DO PROGRAMA.")
      cnx.close()
      text = False
   else:
      print("Objeto digitado não coresponde com nenhuma opção\n" + \
            "TENTE NOVAMENTE.")




```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
