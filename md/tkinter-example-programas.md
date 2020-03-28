---
title: tkinter example programas (snippet)
date: 2020-02-09
tags: ["python"]
---
Python tkinter example 'programas'

Functions in program: 
* `def focusOutE2(event):`
* `def focusInE2(event):`
* `def focusOutE1(event):`
* `def focusInE1(event):`
* `def funcion(x):`
* `def potencia(x,y):`
* `def potencia(base, potencia):`
* `def main():`

Modules used in program: 
* `import os`
* `import os`
* `import os`
* `import os`
* `import os`
* `import Tkinter`
* `import os`
* `import os`
* `import math `
* `import math`
* `import os`
* `import os`
* `import os `
* `import random`
* `import os`

## python programas

Python tkinter example: programas

```python
#!/bin/python
# coding=utf-8




#29.
nombre= "Alondra alcantara"
sexo=2 # mujer=2, hombre=1
no_hijos=2
salario_mensual=15000
bono=0.0
descuento=0.0
if sexo == 2:
	if no_hijos > 0:
		bono=100.0
	else:
		bono=50.0
else:
	if salario_mensual < 600.0:
		bono=40

salario_mensual=salario_mensual + bono
print(salario_mensual)
descuento = (12.5 / 100) * salario_mensual
print(descuento)

print(salario_mensual)
print(nombre)
print(bono)
print(descuento)
print(salario_mensual - descuento)



#28. PyZenity
'''
from PyZenity import InfoMessage
import os
import random


def main():
    ganador=""
    result="JUEGO DEL DISPAREJO\n\n*******************"
    moneda=["SOL","AGUILA"]#simulamos una moneda con dos caras: AGUILA y SOL
    hugo,paco,luis="","",""#definimos tres jugadores

    #obtenemos valores aleatorios
    hugo=moneda[int(random.randrange(0,2))]
    paco=moneda[int(random.randrange(0,2))]
    luis=moneda[int(random.randrange(0,2))]

    result+='\nTurno de Hugo:'+str(hugo)
    result+='\nTurno de Paco:'+str(paco)
    result+='\nTurno de Luis:'+str(luis)

#las condiciones del programa
    if hugo==luis and hugo==paco:
        ganador="Las tres monedas son iguales, No hay ganador"
    elif hugo!=luis and hugo!=paco:
        ganador="El ganador es Hugo"
    elif paco!=luis and paco!=hugo:
        ganador="El ganador es Paco"
    else:
        ganador="El ganador es Luis"

    result+='\n*******************\n'+str(ganador)
    InfoMessage(result)
    os.system('clear')


if __name__ == '__main__':
    main()
'''

#27. PyZenity
'''
from PyZenity import InfoMessage
from PyZenity import Question
import os 

os.system('clear')

dato= Question('Quieres continuar?')
#InfoMessage(dato)
if dato == True:
	InfoMessage("Instalando ...")
else:
	InfoMessage("Cancelando ...")
'''


#26.
'''
MAX=100
c=0
while c <= MAX:
	c= c**(c+1)
	c= c +1
print(c)
'''

#25. 
'''
def potencia(base, potencia):
	i=0
	pot=1
	while i <= potencia:
		pot= pot *base 
		i= i + 1
		print(">>",pot, " >> ", i)
	return pot 

print(potencia(5, 4))
'''


#24.
'''
potencia=1
i=0
b=2
while i <= b:
	potencia= potencia *b
	i= i + 1
	print(">>i=",i)
print("potencia=",potencia)
'''

#23.
''' 
a, b=2, 5
x, y=a, b
p=0
while x != 0:
	x=x - 1
	p= p + y

print("a=",a," , b=",b, " , p=", p)
'''

#22.
'''
import os

os.system('clear')

numero, cont=5, 1
mayor= numero
while cont < 50:
	cont=cont + 1
	numero=int(raw_input("Numero:"))
	if mayor < numero:
		mayor = numero
		break
print(mayor)
'''



#21.
'''
import os

os.system('clear')
tiempo_disponible=True
dinero=True
pagar_examen=False
pasar_examen=False

if tiempo_disponible == True:
	print(">>Tienes tiempo disponible")
	if dinero == True:
		print(">>Bien, tienes el dinero necesario")
		if pagar_examen:
			print(">>Bien, pagaste el examen de admision")
			if pasar_examen == True:
				print(">>Estudia la maestria")
			else:
				print("Necesitas volver a presentar examen")
		else:
			print("Necesitas pagar examen")
	else:
		print("Necesitas conseguir dinero")
else:
	print("Administra mejor tu tiempo")

'''


#20.
'''
l=""
linea="Jacobo paso el examen de logica"
print(linea)

for l in linea:
	print(l)
'''

#19.
'''
import math

n,c=1,0
MAX=10



def potencia(x,y):
	return math.pow(x,y)



while n < MAX:
	n=n+potencia(n,c)**c
	c=c+1
	print(">>c=",c," , >>n=",n )

print("c=",c," , n=",n )
'''

#18. 
'''
import math 

print("math.sqrt(100) : ", math.sqrt(100))
print("math.sqrt(7) : ", math.sqrt(7))
print("math.sqrt(math.pi) : ", math.sqrt(math.pi))

numeros=[1,2,3,4,5,6]
print([ math.sqrt(n) for n in numeros])
'''

#17.
'''
numeros=[1,2,3,4, 5, 6, 7]
print([ x*2 for x in numeros])
print([x**2 for x in numeros])
'''

#16.
'''
def funcion(x):
	return (2*x)+1

x,y=0,0
while x<=10:
	y=funcion(x)
	x=x+1
	print(">>x=",x," , >>y=",y)
print("x=",x," , y=",y)
'''
 

#15.
'''
a,b=0,0.0
print(">>a=",a," , >>b=",b)
while a < 2000:
	a=a*pow(a,b)
	b=b+1
	a=a+1
	print(">>a=",a," , >>b=",b)
	if a < 10:
		print(">>a=",a," , >>b=",b)
		break
print("a=",a," , b=",b)
'''


#14.
'''
x,y=1,1
print(">>x=",x," , >>y=",y)
while y < 100:
	y=3*y
	x=x+0.5
	print(">>x=",x," , >>y=",y)
	if x == 3.0:
		break
print("x=",x," , y=",y)
'''

#13. Bucle infinito, a siempre es cero, b se incrementa
'''
a,b=0,10
print(">>a=",a," , >>b=",b)
while a < b:
	a=a**2
	b=b+1
	print(">>a=",a," , >>b=",b)
print("a=",a," , b=",b)
'''


#12.
'''
x, i=1,1
print(">>i=",i," ,>>x=",x)
while x <= 2000:
	x=2**x
	i=i+1
	print(">>i=",i," ,>>x=",x)
print("i=",i,", x=",x)
'''

#11.
'''
a=9
b=10
if a == b:
	print("Son iguales")
else:
	print("Son diferentes")
'''

#10. PyZenity
'''
import os
os.system('clear')
try:
    import PyZenity
    fecha=PyZenity.GetDate('Por favor selecciona una fecha',selected=False)
    PyZenity.InfoMessage('Fecha seleccionada '+str(fecha))
except ImportError:
    print("necesitas instalar la librería PyZenity")
'''


#9. Tkinter
'''
from Tkinter import *
top = Tk()
L1 = Label(top, text="Usuario")
L1.pack( side = LEFT)
E1 = Entry(top, bd =5)

E1.pack(side = RIGHT)

top.mainloop()
'''

#8. Tkinter
'''
import os
import Tkinter
from Tkinter import *
os.system('clear')
root = Tk()
root.mainloop()
'''

#7. Tkinter
'''
import os

os.system('clear')

try:
	from Tkinter import *
	root = Tk()
	label1 = Label(root,text ="Nombre de usuario")
	label2 = Label(root,text ="Clave de acceso")
	entry1= Entry(root)
	entry2= Entry(root)
	label1.grid(row=0,sticky=E) 
	label2.grid(row=1)
	entry1.grid(row=0,column=1) 
	entry2.grid(row=1,column=1)
	c =Checkbutton(root,text="Introduce tus datos")
	c.grid(columnspan=5)
	root.mainloop()
	
except Exception as e:
	print("Hay un error: ",e)
'''


#6. Que version de Python usas
'''
import os
from sys import version_info

os.system('clear')
if version_info.major == 2:
    import Tkinter as tk
    print("Usas Python 2.x")
elif version_info.major == 3:
    import tkinter as tk
    print("Usas Python 3.x")
'''

#5. tkinter
'''
from tkinter import *
from tkinter import ttk, Canvas, Frame, BOTH, font

root = Tk() # Skapar fönstret

root.title("SoundCloud") # Fönstertitel 

#root.geometry("400x200") # Fönsterstorlek, BREDDxHÖJD
root.withdraw()
root.update_idletasks()  # Update "requested size" from geometry manager

x = (root.winfo_screenwidth() - root.winfo_reqwidth()) / 2
y = (root.winfo_screenheight() - root.winfo_reqheight()) / 2
root.geometry("+%d+%d" % (x, y))

# This seems to draw the window frame immediately, so only call deiconify()
# after setting correct window position
root.deiconify()

root.configure(background = 'white') # Bakgrundsfärg till fönstret

rootFont = font.Font(family='Arial', size=10, weight='bold')

w_width = 400
w_height = 200
w = Canvas(root, width=w_width, height=w_height, # Skapar 'w' canvas't
           borderwidth=2,
           highlightthickness=0,
           background='white')

w.pack() # Vad gör denna? :(

y = int(50)
w.create_line(20, y, w_width-20, y, fill="#ddd", width=2)
w.create_line(20, 100, w_width-20, 100, fill="#ddd", width=2)

signin = Button(w, text="Log In", fg="white", bg="#f70", bd="-2", height="3", width="20", command=w.quit, font=rootFont)
signin.pack(side=LEFT)
signin.place(relx=.75, rely=.75, anchor="c")
#
CheckVar2 = IntVar()
C2 = Checkbutton(root, text = "Remember me", variable = CheckVar2, onvalue = 1, offvalue = 0, height=2, width = 20, bg="white")
C2.pack(side=LEFT)
C2.place(relx=.2, rely=.75, anchor="c")
# Username input
username = StringVar()
username.set("Username")
E1 = Entry(root, bd = 0, width=59, textvariable=username)
E1.pack(side=LEFT)
E1.place(relx=.495, rely=.18, anchor="c")
# Password input
password = StringVar()
password.set('Password')
E2 = Entry(root, bd = 0, width=59, textvariable=password)
E2.pack(side=LEFT)
E2.place(relx=.495, rely=.43, anchor="c")

w.create_line(375, 200, w_width-20, 200, fill="#ddd", width=2)


def focusInE1(event):
    w.create_line(20, y, w_width-20, y, fill="#f70", width=2)
    #username.set("")

def focusOutE1(event):
    w.create_line(20, y, w_width-20, y, fill="#ddd", width=2)
    #username.set("Username")

E1.bind("<FocusIn>", focusInE1)
E1.bind("<FocusOut>", focusOutE1)
#
def focusInE2(event):
    w.create_line(20, 100, w_width-20, 100, fill="#f70", width=2)
    #password.set('')

def focusOutE2(event):
    w.create_line(20, 100, w_width-20, 100, fill="#ddd", width=2)
    #password.set("Password")

E2.bind("<FocusIn>", focusInE2)
E2.bind("<FocusOut>", focusOutE2)

root.mainloop() # Loopar funktionen, för Windows användare endast
'''

#4. Verificando Tkinter
'''
import os

os.system("clear")
try:
	import Tkinter
	print("El paquete esta instalado")


except Exception as e:
	print("Tkinter no instalado ",e)

'''


#3. Mayor de 3 números
'''
a, b, c= 1, 3, 9
print("Programando en Python")
if a > b:
	if a > c:
		print("a= ",a, " es mayor a b= ",b, " y c= ",c)
	else:
		print("c= ",c, " es mayor a a= ",a, " y b= ",b)
else:
	if b > c:
		print("b= ",b, " es mayor a a= ",a, " y c= ",c)
	else:
		print("c= ",c, " es mayor a a= ",a, " y b= ",b)
'''


#2. Usando AWK
'''
import os

try:
  os.system("awk '//{print}' datos.dat")
except:
  print("Error")
'''


'''
#1. Obteniendo fecha
import os
print("Hola, hoy es: ")
os.system("date")
'''


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
