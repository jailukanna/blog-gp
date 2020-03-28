---
title: tkinter example wesley1b (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'wesley1b'

Functions in program: 
* `def resultado():`
* `def float_ou_zero(campo):`

Modules used in program: 
* `import tkinter.messagebox`
* `import tkinter`

## python wesley1b

Python tkinter example: wesley1b

```python
import tkinter
import tkinter.messagebox

janela = tkinter.Tk()

janela.title('Média Aluno')

rotulo = tkinter.Label(janela, text='»»»»» Descubra se você foi aprovado ou reprovado na escola «««««««')
rotulo.grid(row=0, column=0, columnspan=2)

rotulo2 = tkinter.LabelFrame(janela, text="AVISO")
rotulo2.grid(row=1, column=0, columnspan=2, padx=10, pady=10)

rotulo3 = tkinter.Label(rotulo2, text="Antes de começar, lembre-se que não é permitido o uso de vírgulas, apenas pontos")
rotulo3.grid(row=2, column=0, columnspan=2, padx=10, pady=10)

rotulonome = tkinter.Label(janela, text='Primeiro nome do aluno:')  # input»comando tipo pergunta ex:digite alguma coisa
rotulonome.grid(row=3, column=0, sticky=tkinter.E)

camponome = tkinter.Entry()
camponome.grid(row=3, column=1)

rotulom1 = tkinter.Label(janela, text='Média da escola do aluno:')
rotulom1.grid(row=4, column=0, sticky=tkinter.E)

campom1 = tkinter.Spinbox(from_=0, to=10)
campom1.grid(row=4, column=1)

rotulon1 = tkinter.Label(janela, text='Primeira nota:')  # nao precisa informar o tipo da variavel,da o nome e qqr valor
rotulon1.grid(row=5, column=0, sticky=tkinter.E)

campon1 = tkinter.Entry()
campon1.grid(row=5, column=1)

rotulon2 = tkinter.Label(janela, text='Segunda nota:')  # mais precisa declarar a variavel sempre
rotulon2.grid(row=6, column=0, sticky=tkinter.E)

campon2 = tkinter.Entry()
campon2.grid(row=6, column=1)

rotulon3 = tkinter.Label(janela, text='Terceira nota:')
rotulon3.grid(row=7, column=0, sticky=tkinter.E)

campon3 = tkinter.Entry()
campon3.grid(row=7, column=1)

rotulon4 = tkinter.Label(janela, text='Quarta nota:')  # variaveis:int==inteiro:str==string e float
rotulon4.grid(row=8, column=0, sticky=tkinter.E)

campon4 = tkinter.Entry()
campon4.grid(row=8, column=1)


def float_ou_zero(campo):
    try:
        v = float(campo.get())
    except ValueError:
        v = 0.0
    return v


def resultado():
    soma1 = float_ou_zero(campon1) + float_ou_zero(campon2) + float_ou_zero(campon3) + float_ou_zero(campon4)
    soma = soma1 / 4
    if soma <= float_ou_zero(campom1):
       tkinter.messagebox.showinfo('RESULTADO', f'Aluno {camponome.get()} - REPROVADO\nA média do alunoALUNO é: {soma}')
    else:
       tkinter.messagebox.showinfo('RESULTADO', f"Aluno {camponome.get()} - APROVADO \nA média do aluno é: {soma}")

botao = tkinter.Button(janela, text='ENVIAR', command=resultado)
botao.grid(row=9, column=0, columnspan=2, padx=10, pady=10)

janela.mainloop()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
