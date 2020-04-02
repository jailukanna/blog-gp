---
title: PIL example deteccionMovimiento (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'deteccionMovimiento'

Functions in program: 
* `def main(): # rutina principal`
* `def colstr(r, g, b):`
* `def colhex(v):`
* `def convolucion(imagen, h):`
* `def getObjectInfo(grupo, imagen, id):`
* `def deteccionObjetos(imagen, objts):`
* `def asignColor(grupo):`
* `def bfs(imagen, cola, ancho, altura, grupo):`
* `def box(x, y, w, h, color): `
* `def assignColor(grupo):`

Modules used in program: 
* `import time, PIL.Image, ImageDraw, ImageFont`
* `import numpy as np`

## python deteccionMovimiento

Python PIL example: deteccionMovimiento

```python
import numpy as np
import time, PIL.Image, ImageDraw, ImageFont
from PIL import ImageDraw
from math import fabs
from subprocess import call
from Tkinter import * # ventanas
from random import random # pseudoaleatorio
from random import randint
from math import sqrt, log # evidente
from time import sleep # retraso
from sys import argv # parametros de linea de instr.
from threading import Thread # hilos (paralelismo)

def assignColor(grupo):
    return colstr((13 * grupo * 5 + 23) % 256, (3 * grupo * 7 + 31) % 256, (9 * grupo * 13 + 41) % 256)

class Objeto:
    def __init__(self, id, w, h, xc, yc, area, es):
        self.id = id
        self.w = w
        self.h = h
        self.xc = xc
        self.yc = yc
        self.area = area
        self.es = es
        self.direccion = ''
        

class Cuadrado:
    def __init__(self, id, x, y, t, v, d): # constructor
        self.x = x
        self.y = y
        self.t = t
        self.v = v
        self.d = d
        self.dx = 0
        self.dy = 0
        self.dz = 0
        self.counter = 0
        self.color = assignColor(id)
        self.drawing = None

    def step(self): # metodo de instancia
        if self.counter == 0:
            seleccion = randint(0, 3)
            self.dx = 0
            self.dy = 0
            self.dz = 0
            if seleccion == 0:
                self.dx = randint(-self.v, self.v+1)
            elif seleccion == 1:
                self.dy = randint(-self.v, self.v+1)
            else:
                self.dz = randint(-self.v, self.v+1)

        self.counter += 1
        if self.counter >= self.d:
            self.counter = 0

        self.x += self.dx
        self.y += self.dy
        self.t += self.dz

        if self.t < 1:
            self.t = 1
        elif self.t > 100:
            self.t = 100
        if self.x < 0:
            self.x = 0
        elif self.x > ( 700 - self.t ):
            self.x = 700 - self.t
        if self.y < 0:
            self.y = 0
        elif self.y > ( 700 - self.t ):
            self.y = 700 - self.t
        return

    def repaint(self):
        global canvas
        if self.drawing is not None:
            canvas.delete(self.drawing) # quito el viejo
        self.drawing = box(self.x, self.y, self.t, self.t, self.color)
        return

dim = 300
tk = Tk() # controlador
canvas = Canvas(tk, width = dim, height = dim) # ventana
speed = 1

def box(x, y, w, h, color): 
    return canvas.create_rectangle( x+w, y+h, x, y, outline= '#000000', fill = color, width = 1)

class Simulacion(Thread): # clase hijo de la clase hilo (paralelismo)
    def __init__(self, n):
        Thread.__init__(self)
        self.nodes = n
        return

    def run(self):
        global canvas, speed
        objetos = None
        while True:
            for n in self.nodes:
                n.repaint()
            sleep(speed)
            for n in self.nodes:
                n.step()
            canvas.update()
            canvas.postscript(file="image.ps", colormode='color')
            call(["convert","image.ps","image.png"])
            
            t1 = time.time()
            
            imagen = PIL.Image.open('image.png')
            original = imagen
            
            px = np.array([[-1,0,1],[-1,0,1],[-1,0,1]])
            py = np.array([[1,1,1],[0,0,0], [-1,-1,-1]])
            
            g = (convolucion(original, px)**2 + convolucion(original, py) **2) ** 0.5
            
            prom = np.average(g)
            ancho, altura = imagen.size
            imagen = imagen.convert('L')
            im = imagen.load()
            for x in xrange(ancho):
                for y in xrange(altura):
                    if g[x, y] < 4 * prom / 3:
                        im[x, y] = 0
                    else:
                        im[x, y] = 255
            resultado, objetos = deteccionObjetos(PIL.Image.fromarray(np.array(imagen)), objetos)
            t2 = time.time()
            print("Tiempo total: ",t2 -t1)


#metodo breath first search
#toma como parametro la matriz de la imagen, una copia de la matriz
#el color asignado RGB, la cola que es una lista, el ancho y la
#altura de la imagen original
def bfs(imagen, cola, ancho, altura, grupo):
    #toma el primer elemento de la cola y lo saca
    (x, y) = cola.pop(0)
    #si imagen no es color negro nos regresa un false
    if not imagen[x, y] == 0:
        return False
    #toma como blanco el pixel en la cola
    imagen[x, y] = 255 # ignora por poner en blanco
    grupo.append((x, y))
    for dx in [-1, 0, 1]:
        for dy in [-1, 0, 1]:
            (px, py) = (x + dx, y + dy)
            if px >= 0 and px < ancho and py >= 0 and py < altura:
                if imagen[px, py] == 0: # solo los negros entran en la cola
                    if (px, py) not in cola:
                        cola.append((px, py))
    return True

def asignColor(grupo):
    return ((grupo * 5 + 7) % 256, (grupo * 13 + 41) % 256, (grupo * 29 + 13) % 256)

#Metodo para hacer deteccion de objetos
#toma como parametro el nombre de la imagen
def deteccionObjetos(imagen, objts):
    #Toma las proporciones de la imagen
    ancho, altura = imagen.size
    #toma es escala de grises la imagen
    imagen = imagen.convert('L')
    #carga la imagen para manipularla
    im = imagen.load()

    porAsignar = list()
    for x in xrange(ancho):
        for y in xrange(altura):
            if im[x, y] == 255: # blanco
                porAsignar.append((x, y))

    grupos = list()
    while True:
        grupo = list()
        # se coloca como false un marcador
        listo = False
        #Se recorre segun las proporciones
        for y in xrange(altura):
            for x in xrange(ancho):
                # si el pixel actual es negro
                #el marcador se coloca en true
                # y se sale del ciclo
                if im[x, y] == 0: # negro
                    listo = True
                    break
            if listo:
                break
        #si el marcador es falso sale del ciclo infinito
        if not listo:
            break
        #se crea la cola
        cola = list()
        # agrega la coordenada donde se encuentra nuestro pixel
        cola.append((x, y))
        #este ciclo seguira hasta que la cola no tenga nada
        while len(cola) > 0:
            #se hace breath first search
            #tomando como parametro matriz de la imagen, la matriz creada en rgb
            #la cola, el ancho y la altura de la imagen
            bfs(im, cola, ancho, altura, grupo)
        grupos.append(grupo)
    
    mayor = 0
    fondoPos = None
    objetos = list()
    copia = PIL.Image.new(mode = 'RGB', size = (ancho, altura))
    cp = copia.load()
    for pos in xrange(len(grupos)):
        tam = len(grupos[pos])
        if tam > mayor:
            fondoPos = pos
            mayor = tam
    del grupos[fondoPos]
    print('Cantidad de objetos:',len(grupos))
    print('Fondo pos', fondoPos)
    for pos in xrange(len(grupos)):
           print('Crea objeto')
           objetos.append(getObjectInfo(grupos[pos], cp, pos))

    if objts != None and len(objts) == len(objetos):
        f = ImageFont.load_default()
        i = ImageDraw.Draw(copia)
        print('objts :',len(objts),' objetos:', len(objetos))
        for objeto in xrange(len(objts)):
            direccion = 'ID %s '%objts[objeto].id
            dx = objts[objeto].xc - objetos[objeto].xc
            dy = objts[objeto].yc - objetos[objeto].yc
            if objts[objeto].w < objetos[objeto].w:
                print('Objeto %s se acerca'%(objts[objeto].id))
                direccion+=' + '
            elif objts[objeto].w > objetos[objeto].w:
                print('Objeto %s se aleja'%(objts[objeto].id))
                direccion+=' - '
            elif dx > 0:
                print('Objeto %s se mueve a la izquierda'%(objts[objeto].id))
                direccion+=' <- '
            else:
                print('Objeto %s se mueve a la derecha'%(objts[objeto].id))
                direccion+=' -> '
            if dy > 0:
                print('Objeto %s se mueve hacia arriba'%(objts[objeto].id))
                direccion+=' arriba '
            else:
                print('Objeto %s se mueve hacia abajo'%(objts[objeto].id))
                direccion+=' abajo '
            i.text((objetos[objeto].xc, objetos[objeto].yc), direccion, font=f, fill = '#FFFFFF')
    copia.save('objetos.png', option='optimize')

    return PIL.Image.fromarray(np.array(copia)), objetos


def getObjectInfo(grupo, imagen, id):
    global dim
    minX = dim
    maxX = 0
    minY = dim
    maxY = 0
    for (x, y) in grupo:
        imagen[x, y] = asignColor(id)
        if x < minX:
            minX = x
        if x > maxX:
            maxX = x
        if y < minY:
            minY = y
        if y > maxY:
            maxY = y
    ancho = maxX - minX
    altura = maxY - minY
    xc = minX + ancho / 2
    yc = minY + altura / 2
    es = (minX, minY)
    print('Objeto %s esquina superior %s  %s dimensiones ancho %s altura %s centro %s %s '%(id, minX, minY, ancho, altura, xc, yc))
    return Objeto(id, ancho, altura, xc, yc, ancho * altura, es)
                               
                               


def convolucion(imagen, h):
    iancho, ialtura = imagen.size
    imagen = imagen.convert('L')
    im = imagen.load()
    maltura, mancho = h.shape
    g = np.zeros(shape = (iancho, ialtura))

    for x in xrange(iancho):
        for y in xrange(ialtura):

            sum = 0.0
            c = 0.0001

            for i in xrange(mancho):
                zi = ( i - ( mancho / 2 )) 
                for j in xrange(maltura):
                    zj = ( j - ( maltura / 2 ) )

                    if x + zi >= 0 and x + zi < iancho and \
                            y + zj >= 0 and y + zj < ialtura:
                        sum += im[x + zi, y + zj] * h[i, j]
                        c += 1.0

            g[x, y] = sum / c

    return g



def colhex(v):
    s = '%x' % v
    while len(s) < 2:
        s = '0%s' % s
    return s

def colstr(r, g, b):
    return '#%s%s%s' % (colhex(r), colhex(g), colhex(b))

def main(): # rutina principal
    global canvas, dim, speed
    canvas.pack() # crear la ventana en su tamano
    nodes = list()
    try:
        count = int(argv[1])
    except:
        count = 15
    try:
        speed = float(argv[2])
    except:
        speed = 0.2

    for c in xrange(count):
        t = randint(20, 50)
        x = randint(0,dim - t)
        y = randint(0,dim - t)
        v = randint(1, 4)
        d = randint(1, 15)
        nodes.append(Cuadrado(c, x, y, t, v, d))

    s = Simulacion(nodes)
    s.start()
    mainloop()

    return

main()

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
