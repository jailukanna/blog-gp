---
title: tkinter example CALCULADORACDBPREFIXADO (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'CALCULADORACDBPREFIXADO'

Functions in program: 
* `def calcular():`

## python CALCULADORACDBPREFIXADO

Python tkinter example: CALCULADORACDBPREFIXADO

```python
try:
	import tkinter
except ImportError: # python 2
	import Tkinter as tkinter

from tkinter import *

from datetime import date, timedelta

mainWindow = tkinter.Tk()
mainWindow.title("Calculadora de Renda Fixa")
mainWindow.geometry('640x480')

# funcoes de calculo


def calcular():

	# Retornando as datas

	datainicio = date(int(yearinSpin.get()), int(monthinSpin.get()), int(dayinSpin.get()))
	datavencimento = date(int(yearoutSpin.get()), int(monthoutSpin.get()), int(dayoutSpin.get()))

	difference = datavencimento - datainicio
	days = difference.days

	# CALCULO VALOR FINAL

	taxaget = taxa.get()
	taxaget = float(taxaget.replace(',', '.'))
	valoringet = valorin.get()
	valoringet = float(valoringet.replace(',', '.'))
	valorfin.delete(0, END)
	IR.delete(0, END)
	valorli.delete(0, END)
	taxadia = ((1 + (float(taxaget) / 100)) ** (int(days) / 360)) - 1
	calc = (float(taxadia) * float(valoringet)) + float(valoringet)

	valorfin.insert(0, round(calc, 2))

	# CALCULO IR e Valor Liquido
	irfin = ''
	ir225 = round(((round(calc, 2) - float(valoringet)) * 0.225), 2)
	ir20 = round(((round(calc, 2) - float(valoringet)) * 0.20), 2)
	ir175 = round(((round(calc, 2) - float(valoringet)) * 0.175), 2)
	ir15 = round(((round(calc, 2) - float(valoringet)) * 0.15), 2)
	if days <= 180:
		IR.insert(0, ir225)
		irfin = float(ir225)
	elif days > 180 and days <= 360:
		IR.insert(0, ir20)
		irfin = float(ir20)
	elif days > 360 and days <= 720:
		IR.insert(0, ir175)
		irfin = float(ir175)
	elif days > 720:
		IR.insert(0, ir15)
		irfin = float(ir15)
	else:
		IR.insert(0, 0)

	# CALCULO Valor Liquido
	calcli = calc - irfin
	valorli.insert(0, round(calcli, 2))

	return


# TÍTULO
title = tkinter.Label(mainWindow, text='CDB Pré-Fixado')
title.grid(row=0, column=0, columnspan=3)

# Taxas
taxaLabel = tkinter.Label(mainWindow, text="Taxa Fixa")
taxaLabel.grid(row=1, column=0, sticky='w', columnspan=3)
taxa = tkinter.Entry(mainWindow)
taxa.grid(row=2, column=0, sticky='ew', columnspan=3)
taxap = tkinter.Label(mainWindow, text='% a.a.')
taxap.grid(row=2, column=4, sticky='w')

# Date spinners
datainLabel = tkinter.Label(mainWindow, text="Data Inicial")
datainLabel.grid(row=3, column=0, sticky='w', columnspan=3)

dataoutLabel = tkinter.Label(mainWindow, text="Vencimento")
dataoutLabel.grid(row=3, column=5, sticky='w', columnspan=3)

dayinSpin = tkinter.Spinbox(mainWindow, width=5, from_=1, to=31)
monthinSpin = tkinter.Spinbox(mainWindow, width=5, values=(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12))
yearinSpin = tkinter.Spinbox(mainWindow, width=5, from_=2000, to_=3000)
dayinSpin.grid(row=4, column=0)
monthinSpin.grid(row=4, column=1)
yearinSpin.grid(row=4, column=2)
dayoutSpin = tkinter.Spinbox(mainWindow, width=5, from_=1, to=31)
monthoutSpin = tkinter.Spinbox(mainWindow, width=5, values=(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12))
yearoutSpin = tkinter.Spinbox(mainWindow, width=5, from_=2000, to_=3000)
dayoutSpin.grid(row=4, column=5)
monthoutSpin.grid(row=4, column=6)
yearoutSpin.grid(row=4, column=7)

# Valor Inicial

valorinLabel = tkinter.Label(mainWindow, text="Valor Inicial")
valorinLabel.grid(row=5, column=0, sticky='w', columnspan=3)
valorin = tkinter.Entry(mainWindow)
valorin.grid(row=6, column=0, sticky='ew', columnspan=3)

# Botao calcular

botaocalcular = tkinter.Button(mainWindow, text='Calcular', command=calcular)
botaocalcular.grid(row=7, column=0, columnspan=3)

# Valor Final, IR, Valor Líquido

valorfinLabel = tkinter.Label(mainWindow, text="Valor Final")
valorfinLabel.grid(row=8, column=0, sticky='w', columnspan=3)
valorfin = tkinter.Entry(mainWindow)
valorfin.grid(row=9, column=0, sticky='ew', columnspan=3)
IRLabel = tkinter.Label(mainWindow, text="Imposto de Renda")
IRLabel.grid(row=8, column=4, sticky='w', columnspan=3)
IR = tkinter.Entry(mainWindow)
IR.grid(row=9, column=4, sticky='ew', columnspan=3)
valorliLabel = tkinter.Label(mainWindow, text="Valor Liquido")
valorliLabel.grid(row=8, column=7, sticky='w', columnspan=3)
valorli = tkinter.Entry(mainWindow)
valorli.grid(row=9, column=7, sticky='ew', columnspan=3)

mainWindow.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
